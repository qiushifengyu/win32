---
Description: The Notify method notifies the pin that a quality change is requested. This method implements the IQualityControl::Notify method.
ms.assetid: 76124321-0d2d-4fee-a08a-4db23078e8df
title: CBaseInputPin.Notify method
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# CBaseInputPin.Notify method

The `Notify` method notifies the pin that a quality change is requested. This method implements the [**IQualityControl::Notify**](/windows/desktop/api/Strmif/nf-strmif-iqualitycontrol-notify) method.

## Syntax


```C++
HRESULT Notify(
   IBaseFilter *pSelf,
   Quality     q
);
```



## Parameters

<dl> <dt>

*pSelf* 
</dt> <dd>

Pointer to the filter that is sending the quality-control message.

</dd> <dt>

*q* 
</dt> <dd>

[**Quality**](/windows/desktop/api/strmif/ns-strmif-tagquality) structure that contains the quality-control message.

</dd> </dl>

## Return value

Returns S\_OK.

## Remarks

Filters usually pass quality-control messages to an upstream output pin, not to an input pin. Therefore, this method returns S\_OK without doing anything.

## Requirements



|                    |                                                                                                                                                                                            |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Header<br/>  | <dl> <dt>Amfilter.h (include Streams.h)</dt> </dl>                                                                                  |
| Library<br/> | <dl> <dt>Strmbase.lib (retail builds); </dt> <dt>Strmbasd.lib (debug builds)</dt> </dl> |



## See also

<dl> <dt>

[**CBaseInputPin Class**](cbaseinputpin.md)
</dt> <dt>

[Quality-Control Management](quality-control-management.md)
</dt> </dl>

 

 



