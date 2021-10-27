
```csharp
#region Consume Service

//Parameters
GetOrderServiceWCF.Parametros  parameters = new  GetOrderServiceWCF.Parametros ();
parameters.SenderId = _configuration.GetSection ("ConsumeServiceData:SenderId").Value;
parameters.SenderQualifier = _configuration.GetSection ("ConsumeServiceData:SenderQualifier").Value;
parameters.DocumentType = _configuration.GetSection ("ConsumeServiceData:DocumentType").Value;
parameters.Markedsent = bool.Parse (_configuration.GetSection ("ConsumeServiceData:Markedsent").Value);

//Get info from service
string  urlGetSrvice = _configuration.GetSection ("UrlService:GetOrdersService").Value;
GetOrderServiceWCF.GetOrdersServiceClient  client = new  GetOrderServiceWCF.GetOrdersServiceClient(new GetOrderServiceWCF.GetOrdersServiceClient.EndpointConfiguration (), urlGetSrvice);
Task<GetOrderServiceWCF.getOrderResponse> responseGetOrder = client.getOrderAsync (parameters);
GetOrderServiceWCF.getOrderResponse  getOrderResponse = responseGetOrder.Result;

#endregion
```
