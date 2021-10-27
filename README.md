
```csharp
public GetAllOrdersOut GetAll(GetAllOrdersIn input, UserBE userBE) 
{ 
    #region data base operations 
    Expression<Func<OrderBE, bool>> iquerable = SetEntityFilters(input?.ListItemFilters, userBE); 
    #endregion 
  
    OrderDA orderDA = new OrderDA(); 
    return new GetAllOrdersOut() 
    { 
        ListResult = orderDA.Get(iquerable, input?.Page, input?.ToShow) 
    }; 
} 
```
