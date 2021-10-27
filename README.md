
```csharp
OrderDA orderDA = new OrderDA ();
//validate change status add log 
OrderBE currentData = input.Id > 0 ? orderDA.GetFirstOrDefault (x => x.Id.Equals (input.Id)) : input;
if (input.StatusId != currentData.StatusId || input.Id == 0) {
    input.LogOrderStatus = input.LogOrderStatus == null ? new List<LogOrderStatusBE> () : input.LogOrderStatus;
    input.LogOrderStatus.Add (new LogOrderStatusBE () {
        OrderStatusId = input.StatusId,
            CreationDate = DateTime.Now,
    });
}
//end change status 

OrderBE orderBE = orderDA.InsertOrUpdate (input);

#region update or insert childs 
//Visit 
if (input.Visits != null && input.Visits.Any ()) {
    VisitDA visitDA = new VisitDA ();
    ImageDA imageDA = new ImageDA ();
    EvidenceDA evidenceDA = new EvidenceDA ();
    foreach (var itemVisit in input.Visits) {
        itemVisit.OrderId = orderBE.Id;
        visitDA.InsertOrUpdate (itemVisit);
        foreach (var itemEvidence in itemVisit.Evidences) {
            itemEvidence.VisitId = itemVisit.Id;
            if (itemEvidence.Image != null) {
                if (itemEvidence.Image.File != null) {
                    itemEvidence.Image.Url = FileHelper.SaveFileInBlob 
                    ($"DerikuDeliveryApp/Files/{itemVisit.OrderId}/evidences/{(Guid.NewGuid() + ".jpg")}",
                    itemEvidence.Image.File, _iConfiguration["BlobConfiguration:ConnectionString"], 
                    _iConfiguration["BlobConfiguration:BlobContainer"],
                    _iConfiguration["BlobConfiguration:PathStorage"]);
                }
                imageDA.InsertOrUpdate (itemEvidence.Image);
                itemEvidence.ImageId = itemEvidence.Image.Id;
            }
            evidenceDA.InsertOrUpdate (itemEvidence);
        }
    }
}

if (input.LogOrderStatus != null && input.LogOrderStatus.Any ()) {
    LogOrderStatusDA logOrderStatus = new LogOrderStatusDA ();
    foreach (var item in input.LogOrderStatus) {
        item.OrderId = input.Id;
        logOrderStatus.InsertOrUpdate (item);
    }
}
//update client 
if (currentData.Client != input.Client) {
    ClientDA clientDA = new ClientDA ();
    clientDA.InsertOrUpdate (input.Client);
}

#endregion 
```
