
```typescript
asignCourier() {
    this.showLoader = true;
    this.itemSelected.userId = this.formAsignCourier.controls.user.value;
    this.itemSelected.assignDateTime = new Date(this.formAsignCourier.controls.assignDateTime.value + ' ' 
    + this.formAsignCourier.controls.assignTime.value);
    this.itemSelected.statusId = this.listOrderStatus.filter(x => x.name == "Asignado")[0].id;
    let updateOrderIn: InsertOrUpdateOrderIn = this.itemSelected;
    this.orderService.insertOrUpdateOrder(updateOrderIn).subscribe(data => {
        if (data.result == Result.Success) {
            this.assigNewDataOrder(data);
            this.toastr.success("Se asigno correctamente");
            this.itemSelected = null;
            this.modalService.dismissAll();
        }
        this.showLoader = false;
    });
}
```
