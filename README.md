
```csharp
#region Mapp orderbe from service 
//Initialice object 
orderBE = new OrderBE(); 
orderBE.Order = order; 

orderBE.CorrelationId = ExtractNodeInfomration(doc, "/CrearOrden/WSRequestHeader2/System/correlationID"); 
orderBE.Cun = ExtractNodeInfomration(doc, "/CrearOrden/Orden/cun"); 
orderBE.Type = ExtractNodeInfomration(doc, "/CrearOrden/Orden/tipo"); 
orderBE.CollectionValue = ExtractNodeInfomration(doc, "/CrearOrden/Orden/recaudo/valor"); 
orderBE.Address = ExtractNodeInfomration(doc, "/CrearOrden/Orden/direccion/direccion"); 
orderBE.Neighborhood = ExtractNodeInfomration(doc, "/CrearOrden/Orden/direccion/barrio"); 
orderBE.City = !string.IsNullOrEmpty(orderBE.Type) && orderBE.Type.Equals("Express") ? "11001" : ""; 
orderBE.Department = !string.IsNullOrEmpty(orderBE.Type) && orderBE.Type.Equals("Express") ? "11" : ""; 
orderBE.Names = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/nombres"); 
orderBE.Surnames = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/apellidos"); 
orderBE.TypeDoc = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/tipoDocumento"); 
orderBE.Document = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/numeroDocumento"); 
orderBE.Mobile = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/telefonoMovil"); 
orderBE.Mobile2 = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/telefonoMovil2"); 
orderBE.Phone = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/telefonoFijo"); 
orderBE.Email = ExtractNodeInfomration(doc, "/CrearOrden/Orden/contacto/correo"); 
orderBE.Cellar = ExtractNodeInfomration(doc, "/CrearOrden/Orden/item/bodega"); 
orderBE.Material = ExtractNodeInfomration(doc, "/CrearOrden/Orden/item/material"); 
orderBE.Name = ExtractNodeInfomration(doc, "/CrearOrden/Orden/item/nombre"); 
orderBE.Campaign = ExtractNodeInfomration(doc, "/CrearOrden/Orden/item/campana"); 
orderBE.ProcessDate = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss"); 
orderBE.Status = _configuration.GetSection("SaveOrderData:Status").Value; 
orderBE.Okay = _configuration.GetSection("SaveOrderData:Okay").Value; 
#endregion 

int reuslt = Add(orderBE); 
```
