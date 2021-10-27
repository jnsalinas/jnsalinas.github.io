
```csharp
async Task _UpdateOrder () {
    if (_userPreferencesDependency.GetStoredIntValue ("OrderTransito") > 0 && OrderStatus.Name == "En Transito") {
        await App.Current.MainPage.DisplayAlert ("Alerta", "Verifique aun tiene una orden en transito ", "OK");
        return;
    }
    if (OrderStatus.Name == "Confirmar Entrega") {
        await NavigationService.NavigateToMaterAsync<ConfirmOrderViewModel> (Order);
        return;
    }
    IsEnabledSave = false;
    IsSave = true;
    Order.StatusId = OrderStatus.Id;
    InsertOrUpdateOrderIn insertOrUpdateOrderIn = _mapper.Map<InsertOrUpdateOrderIn> (Order);
    BaseInsertOrUpdateOut baseInsertOrUpdate = await _orderWebServiceRepository.InserOrUpdateOrder (insertOrUpdateOrderIn);
    if (baseInsertOrUpdate.Result == Result.Success) {
        if (OrderStatus.Name == "En Transito") {
            _userPreferencesDependency.StoreIntValue ("OrderTransito", Order.Id);
            await Task.Delay (2000);
            RegisterLocation (Order.Id);
            DependencyService.Get<ILocationService> ().Start ();

        }
        IsSave = false;
        DependencyService.Get<IMessage> ().ShortAlert ("Se actualizo correctamente la orden");
        await _NavigateListOrders ();
        return;
    }
    IsEnabledSave = true;
    IsSave = false;
    DependencyService.Get<IMessage> ().ShortAlert ("Ocurrio un error intente nuevamente o comuniquese con el administrador");
}
```
