
```csharp
OrderBE currentData = input.Id > 0 ? orderDA.GetFirstOrDefault (x => x.Id.Equals (input.Id)) : input;
if (input.StatusId != currentData.StatusId || input.Id == 0) {
    input.LogOrderStatus = input.LogOrderStatus == null ? new List<LogOrderStatusBE> () : input.LogOrderStatus;
    input.LogOrderStatus.Add (new LogOrderStatusBE () {
        OrderStatusId = input.StatusId,
            CreationDate = DateTime.Now,
    });
}
```
