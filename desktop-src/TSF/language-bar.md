---
title: Language Bar
description: Language Bar
ms.assetid: 82b92567-fdc1-488c-b395-4cb95257955c
keywords:
- Text Services Framework (TSF),language bar
- TSF (Text Services Framework),language bar
- text services,language bar
- language bar
- Text Services Framework (TSF),buttons
- TSF (Text Services Framework),buttons
- text services,buttons
- Text Services Framework (TSF),menus
- TSF (Text Services Framework),menus
- text services,menus
- button styles
- menu buttons
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Language Bar

## Implementing the Language Bar Object

To support adding an item to the language bar, a text service must implement an object that supports the [ITfSource](https://msdn.microsoft.com/library/windows/desktop/ms628941) interface and one of the [ITfLangBarItem](https://msdn.microsoft.com/library/windows/desktop/ms628701) control elements. When the item is installed, the language bar installs an [ITfLangBarItemSink](https://msdn.microsoft.com/library/windows/desktop/ms628736) sink by calling the item's [ITfSource::AdviseSink](https://msdn.microsoft.com/library/windows/desktop/ms628945) with IID\_ITfLangBarItemSink. The item uses the **ITfLangBarItemSink** interface to notify the language bar of changes, for example when the item is hidden, shown, enabled, or disabled.

Four types of language bar items can be installed and each of the required interfaces is created from **ITfLangBarItem**. The following are possible **ITfLangBarItem** control elements.



|               |                                                                                                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Button        | A language bar button functions as a command button, toggle control, or a menu on the language bar. The object must support the ITfLangBarItemButton interface.                   |
| Balloon       | A language bar balloon functions as a pop-up notification on the language bar. The object must support the ITfLangBarItemBalloon interface.                                       |
| Bitmap        | A language bar bitmap functions as a static element on the language bar that displays a bitmap. The object must support the ITfLangBarItemBitmap interface.                       |
| Bitmap Button | A language bar bitmap button functions as a button element on the language bar that displays text and a bitmap. The object must support the ITfLangBarItemBitmapButton interface. |



 

## Button Styles

A button element can function as any of the following. The function of the button item is determined by the flags set in the **dwStyle** member of the [TF\_LANGBARITEMINFO](https://msdn.microsoft.com/library/windows/desktop/ms629074) structure in the [ITfLangBarItem::GetInfo](https://msdn.microsoft.com/library/windows/desktop/ms628741) method.



|               |                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Button        | The button functions as a standard command button. This button style is identified by the TF\_LBI\_STYLE\_BTN\_BUTTON style. ITfLangBarItemButton::OnClick is called when the item is clicked. ITfLangBarItemButton::InitMenu and ITfLangBarItemButton::OnMenuSelect are not used.                                                                                                   |
| Toggle Button | The button functions as a toggle control that can maintain a clicked state, similar to a check box. This button style is identified by the TF\_LBI\_STYLE\_BTN\_TOGGLE style. ITfLangBarItemButton::OnClick is called when the item is clicked. ITfLangBarItemButton::InitMenu and ITfLangBarItemButton::OnMenuSelect are not used.                                                  |
| Menu          | The button functions as a drop-down menu. This button style is identified by the TF\_LBI\_STYLE\_BTN\_MENU style. ITfLangBarItemButton::InitMenu is called when the button is clicked. When the user selects an item in the menu, the language bar calls ITfLangBarItemButton::OnMenuSelect with the identifier of the selected menu item. ITfLangBarItemButton::OnClickis not used. |



 

## Implementing a Menu Button

When the user clicks a menu button, the language bar calls [ITfLangBarItemButton::InitMenu](/windows/desktop/api/Ctfutb/nf-ctfutb-itflangbaritembutton-initmenu). The item adds items to the menu using the [ITfMenu](https://msdn.microsoft.com/library/windows/desktop/ms628780) interface passed to InitMenu.

To add a submenu to the menu, call [ITfMenu::AddMenuItem](/windows/desktop/api/Ctfutb/nf-ctfutb-itfmenu-addmenuitem) with TF\_LBMENUF\_SUBMENU. When this is done, a new **ITfMenu** object that represents the submenu is returned in the *ppMenu* parameter of AddMenuItem. This new menu object is used to add items to the submenu.

When the user selects an item in the menu, the language bar calls [ITfLangBarItemButton::OnMenuSelect](/windows/desktop/api/Ctfutb/nf-ctfutb-itflangbaritembutton-onmenuselect) with the identifier of the selected menu item.

## Adding Items to the Language Bar

A text service should add its items to the language bar when its [ITfTextInputProcessor::Activate](https://msdn.microsoft.com/library/windows/desktop/ms628965) method is called and remove them when its [ITfTextInputProcessor::Deactivate](https://msdn.microsoft.com/library/windows/desktop/ms628966) is called.

To add an item to the language bar, the text service obtains an [ITfLangBarItemMgr](https://msdn.microsoft.com/library/windows/desktop/ms628723) interface by calling ITfThreadMgr::QueryInterface with IID\_ITfLangBarItemMgr. The text service then calls [ITfLangBarItemMgr::AddItem](https://msdn.microsoft.com/library/windows/desktop/ms628724) with the pointer to the language bar item object.

The text service must remove the item when deactivated. The text service either uses the same **ITfLangBarItemMgr** interface used to add the items or obtains another instance of the interface. The text service then calls [ITfLangBarItemMgr::RemoveItem](https://msdn.microsoft.com/library/windows/desktop/ms628733) with the interface pointer of the item to remove.

## Extending System Language Bar Items

TSF provides the ability to add menu items to existing language bar menus. This enables a text service to add items to the menu of another text service without having to add a separate button to the toolbar. This also enables the menu items to be organized into logical groups. For example, a text service that provides additional features to the standard speech text service can add items to the speech text service menu rather than adding its own top-level menu button.

A text service provides a language bar menu extension by implementing an object that supports the [ITfSystemLangBarItemSink](https://msdn.microsoft.com/library/windows/desktop/ms628957) interface. This interface works exactly like the [ITfLangBarItemButton](/windows/desktop/api/Ctfutb/nn-ctfutb-itflangbaritembutton) interface for a menu button. When the menu is displayed, the text service being extended calls [ITfSystemLangBarItemSink::InitMenu](https://msdn.microsoft.com/library/windows/desktop/ms628958). The extension adds items to the menu using the [ITfMenu](https://msdn.microsoft.com/library/windows/desktop/ms628780) interface passed to **InitMenu**. When the user selects an item added by the extension, the text service being extended calls [ITfSystemLangBarItemSink::OnMenuSelect](https://msdn.microsoft.com/library/windows/desktop/ms628959) with the identifier of the selected menu item.

To install a language bar menu extension, the text service completes the following steps.

1.  Obtain the **ITfLangBarItem** interface for the item to extend by calling [ITfLangBarItemMgr::GetItem](https://msdn.microsoft.com/library/windows/desktop/ms628728) with the **GUID** for the item to be extended.
2.  Obtain the **ITfSource** interface for the item to extend by calling ITfLangBarItem::QueryInterface with IID\_ITfSource.
3.  Call [ITfSource::AdviseSink](https://msdn.microsoft.com/library/windows/desktop/ms628945) with IID\_ITfSystemLangBarItemSink and the pointer to the **ITfSystemLangBarItemSink** object. If ITfSource::AdviseSink fails, the text service does not support menu extensions.

[ITfSource::UnadviseSink](https://msdn.microsoft.com/library/windows/desktop/ms628946)**ITfSource::AdviseSink**

## Supporting Language Bar Menu Extensions

A text service can enable other text services to add items to its language bar menus as shown above. The text service that must publish its GUID so that the item can be obtained by calling [ITfLangBarItemMgr::GetItem](https://msdn.microsoft.com/library/windows/desktop/ms628728).

To support menu extensions, the text service must support the [ITfSource](https://msdn.microsoft.com/library/windows/desktop/ms628941) interface. The following steps enable support for one or more menu extensions.

1.  When [ITfSource::AdviseSink](https://msdn.microsoft.com/library/windows/desktop/ms628945) with IID\_ITfSystemLangBarItemSink is called, the text service must store the **ITfSystemLangBarItemSink** interface and return a cookie value that will identify the extension.
2.  When [ITfLangBarItemButton::InitMenu](/windows/desktop/api/Ctfutb/nf-ctfutb-itflangbaritembutton-initmenu) is called, the text service calls the extension's [ITfSystemLangBarItemSink::InitMenu](https://msdn.microsoft.com/library/windows/desktop/ms628958) method. The text service must implement a way to identify the menu items added by the extension as opposed to the items added by the text service itself.
3.  When [ITfLangBarItemButton::OnMenuSelect](/windows/desktop/api/Ctfutb/nf-ctfutb-itflangbaritembutton-onmenuselect) is called with a menu item identifier that belongs to an extension, the text service calls the extensions's **ITfSystemLangBarItemSink::OnMenuSelect** method.
4.  When [ITfSource::UnadviseSink](https://msdn.microsoft.com/library/windows/desktop/ms628946) is called with the appropriate cookie, the text service removes the menu extension.

## Related topics

<dl> <dt>

[How To Set Up Text Services Framework](how-to-set-up-tsf.md)
</dt> <dt>

[Language Bar (Applications)](language-bar-app.md)
</dt> </dl>

 

 



