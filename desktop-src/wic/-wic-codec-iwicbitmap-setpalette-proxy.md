---
Description: Proxy function for the SetPalette method.
ms.assetid: 3fd60833-7f21-4654-883a-2dd88c403bc8
title: IWICBitmap\_SetPalette\_Proxy function
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# IWICBitmap\_SetPalette\_Proxy function

Proxy function for the [**SetPalette**](/windows/desktop/api/Wincodec/nf-wincodec-iwicbitmap-setpalette) method.

## Syntax


```C++
HRESULT IWICBitmap_SetPalette_Proxy(
  _In_ IWICBitmap  *THIS_PTR,
  _In_ IWICPalette *pIPalette
);
```



## Parameters

<dl> <dt>

*THIS\_PTR* \[in\]
</dt> <dd>

Type: **[**IWICBitmap**](/windows/desktop/api/Wincodec/nn-wincodec-iwicbitmap)\***

Pointer to this [**IWICBitmap**](/windows/desktop/api/Wincodec/nn-wincodec-iwicbitmap) object.

</dd> <dt>

*pIPalette* \[in\]
</dt> <dd>

Type: **[**IWICPalette**](/windows/desktop/api/Wincodec/nn-wincodec-iwicpalette)\***

The palette to use for conversion.

</dd> </dl>

## Return value

Type: **HRESULT**

If this function succeeds, it returns **S\_OK**. Otherwise, it returns an **HRESULT** error code.

## Remarks

## Requirements



|                                     |                                                                                                                                                                  |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows XP with SP2, Windows Vista \[desktop apps only\]<br/>                                                                                              |
| Minimum supported server<br/> | Windows Server 2008 \[desktop apps only\]<br/>                                                                                                             |
| DLL<br/>                      | <dl> <dt>Windowscodecs.dll; </dt> <dt>Wincodec.lib</dt> </dl> |



 

 



