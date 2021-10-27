
```typescript
getOrders() {
    this.showLoader = true;
    let listItemFilters: ItemFilterVM[] = [];
    if (this.formFilterOrder.controls.guideNumber.value) {
        listItemFilters.push({
            name: "OrderSerial",
            value: this.formFilterOrder.controls.guideNumber.value
        });
    }

    if (this.formFilterOrder.controls.orderSerial.value) {
        listItemFilters.push({
            name: "OrderSerial",
            value: this.formFilterOrder.controls.guideNumber.value
        });
    }

    if (this.formFilterOrder.controls.statusId.value && this.formFilterOrder.controls.statusId.value != "0") {
        listItemFilters.push({
            name: "StatusId",
            value: this.formFilterOrder.controls.statusId.value
        });

        this.formFilterOrder.controls.statusName.setValue(
        this.listOrderStatus.filter(x => x.id == this.formFilterOrder.controls.statusId.value)[0].name);
    } else {
        this.formFilterOrder.controls.statusName.setValue("");
    }

    let filterGetOrder: GetAllOrdersIn = {
        listItemFilters: listItemFilters,
        page: 1,
        toShow: 10
    }

    this.orderService.getOrder(filterGetOrder).subscribe(data => {
        this.listOrder = data.listResult;
        this.showLoader = false;
    });
}
```
