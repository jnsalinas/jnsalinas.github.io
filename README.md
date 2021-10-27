
```csharp
async Task _GetOrders() 
{ 
    IsRefreshing = true; 
    ListOrders = new ObservableCollection<OrderVM>(); 
    List<ItemFilterVM> ListItemFilters = new List<ItemFilterVM>(); 
    ItemFilterVM itemFilter = new ItemFilterVM 
    { 
        Name = "OrderStatus.Name", 
        Value = StatusOrder.GetFilter() 
    }; 

    ListItemFilters.Add(itemFilter); 
    GetAllOrdersIn getAllOrdersIn = new GetAllOrdersIn { ListItemFilters = ListItemFilters }; 
    GetAllOrdersOut getAllOrdersOut = await _orderWebServiceRepository.GetAllOrders(getAllOrdersIn); 
    if (getAllOrdersOut.Result != Result.Success) 
    { 
        List<Expression<Func<OrderBE, object>>> includes = new List<Expression<Func<OrderBE, object>>>(); 
        Expression<Func<OrderBE, object>> expressionUserType; 
        expressionUserType = orderBE => orderBE.Client; 
        includes.Add(expressionUserType); 
        expressionUserType = orderBE => orderBE.OrderStatus; 
        includes.Add(expressionUserType); 
        ListOrders = _mapper.Map<ObservableCollection<OrderVM>>(await _orderDA.GetAll(includes)); 
        IsRefreshing = false; 
        return; 
    } 

    ListOrders = _mapper.Map<ObservableCollection<OrderVM>>(getAllOrdersOut.ListResult); 
    IsRefreshing = false; 
    await _UpdateOrdersLocal(getAllOrdersOut.ListResult).ConfigureAwait(false); 
} 
```
