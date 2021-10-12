<template>
    <div>
        <section class="section-shoppingCart">
            <div class="container card pt-3">
                <div class="row">
                    <div class="col-12 col-sm-12">
                        <p>
                            {{ __('shopping_title') }}
                        </p>
                    </div>
                </div>
                <hr class="dividerLine">
                <div v-for="(orderMeal, mealIndex) in order[0]"
                     v-bind:id="'item_' + mealIndex"
                     v-bind:key="'item' + mealIndex">
                    <div v-for="subMealIndex in orderMeal.amount">
                        <div v-bind:id="'meal_' + mealIndex + '_' + (subMealIndex-1)">
                            <div class="row">
                                <div class="col-12 col-sm-12">
                                    <div class="row">
                                        <div class="col-3">
                                            <img alt="image" class="foodImg lazysize rounded"
                                                 v-bind:src="'\\images\\uploads\\mealAvatar\\' + orderMeal.order_image"
                                                 @error="$event.target.src='\\images\\uploads\\mealAvatar\\default-meal.jpg'">
                                        </div>
                                        <div class="col-9 pl-2 align-self-center">
                                            <div class="row">
                                                <div class="col-6 align-self-center">
                                                    <p class="font-weight-bold">
                                                        {{ orderMeal.name }} #{{ subMealIndex }}
                                                    </p>
                                                    <p>
                                                        {{ orderMeal.order_description }}</p>
                                                    <p class="text-success">
                                                        {{ __('shopping_duration') }}
                                                    </p>
                                                </div>
                                                <div class="col-3 align-self-center">
                                                    <p class="font-weight-bold text-right">{{ orderMeal.price }}€</p>
                                                </div>
                                                <div class="col-3">

                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row mt-2">
                                <div class="col-12 col-sm-12">
                                    <div v-bind:id="'itemIngredient_' + mealIndex"
                                         :class="itemCollapsed[mealIndex] ? 'collapse show' : 'collapse'">
                                        <form action="#" class="form">
                                            <div
                                                v-for="(orderIngredient, ingredientIndex) in order[1][mealIndex][subMealIndex-1]"
                                                v-bind:key="'orderIngredient' + ingredientIndex" class="form-row">
                                                <div class="col-5">
                                                    <div class="float-left">
                                                        <p>{{ orderIngredient.name }}</p>
                                                    </div>
                                                </div>
                                                <div class="col-2 align-self-center">
                                                    <div class="row">
                                                        <div class="col-4 col-sm-4 align-self-center text-center">
                                                            <button :disabled="alreadyOrdered"
                                                                    class="btn btn-outline-info btn-sm"
                                                                    type="button"
                                                                    @click="decrementIngredientAmount(mealIndex, (subMealIndex-1), ingredientIndex)">
                                                                -
                                                            </button>
                                                        </div>
                                                        <div class="col-4 col-sm-4 align-self-center text-center">
                                                            <p v-bind:id="'ingredientAmount_' + mealIndex + '_' + (subMealIndex-1) + '_' + ingredientIndex">
                                                                {{
                                                                    ingredientAmounts[mealIndex][subMealIndex - 1][ingredientIndex]
                                                                }}</p>
                                                        </div>
                                                        <div class="col-4 col-sm-4 align-self-center text-center">
                                                            <button :disabled="alreadyOrdered"
                                                                    class="btn btn-outline-info btn-sm"
                                                                    type="button"
                                                                    @click="incrementIngredientAmount(mealIndex, (subMealIndex-1), ingredientIndex)">
                                                                +
                                                            </button>
                                                        </div>
                                                    </div>
                                                </div>
                                                <div class="col-2">
                                                    <div class="float-right">
                                                            <span
                                                                v-bind:id="'ingredientPrice_' + mealIndex + '_' + (subMealIndex-1) + '_' + ingredientIndex"
                                                                class="badge badge-info badge-size">{{
                                                                    orderIngredient.price
                                                                }}</span>
                                                    </div>
                                                </div>
                                                <div class="col-3">
                                                    <div class="float-right">
                                                        <input
                                                            v-bind:id="'checkBox_' + mealIndex + '_' + (subMealIndex-1) + '_' + ingredientIndex"
                                                            :disabled="alreadyOrdered"
                                                            checked="checked"
                                                            class="checkClass1"
                                                            type="checkbox"
                                                            @click="checkIngredient(mealIndex, (subMealIndex-1), ingredientIndex)">
                                                        <label class="checkClass2"
                                                               v-bind:for="'checkBox_' + mealIndex + '_' + (subMealIndex-1) + '_' + ingredientIndex">
                                                            <span class="ui"></span>
                                                        </label>
                                                    </div>
                                                </div>
                                            </div>
                                        </form>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div v-bind:id="'mealInject_' + mealIndex">

                    </div>
                    <hr class="dividerLineDotted">
                    <div class="row">
                        <div class="col-3 col-sm-3 text-center">

                        </div>
                        <div class="col-4 col-sm-3 text-center">
                            <p>
                                {{ __('shopping_item_amount') }}
                            </p>
                        </div>
                        <div class="col-3 col-sm-3 text-right">
                            <p>
                                {{ __('shopping_item_totalText') }}
                                <wbr>
                                (€)
                            </p>
                        </div>
                        <div class="col-3 col-sm-3 text-center">

                        </div>
                    </div>
                    <hr class="dividerLineDotted">
                    <div class="row">
                        <div class="col-4 col-sm-3 align-self-center">
                            <button class="btn btn-outline-info btn-sm"
                                    type="button"
                                    v-bind:data-target="'#itemIngredient_' + mealIndex"
                                    @click="ingredientCollapse(mealIndex)">
                                <span class="ingredientChoice">
                                    {{ __('shopping_item_ingredientSelect') }}
                                </span>
                            </button>
                        </div>
                        <div class="col-3 col-sm-3 align-self-center">
                            <div class="row">
                                <div class="col-4 col-sm-4 align-self-center text-center">
                                    <button :disabled="alreadyOrdered"
                                            class="btn btn-outline-info btn-sm"
                                            type="button"
                                            @click="deleteMeal(mealIndex)">
                                        -
                                    </button>
                                </div>
                                <div class="col-4 col-sm-4 align-self-center text-center">
                                    <p v-bind:id="'mealAmount_' + mealIndex">{{ orderMeal.amount }}</p>
                                </div>
                                <div class="col-4 col-sm-4 align-self-center text-center">
                                    <button :disabled="alreadyOrdered"
                                            class="btn btn-outline-info btn-sm"
                                            type="button"
                                            @click="addMeal(mealIndex)">
                                        +
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div class="col-3 col-sm-3 align-self-center">
                            <p v-bind:id="'mealPrice_' + mealIndex"
                               class="font-weight-bold text-right">{{ itemPrices[mealIndex] }}</p>
                        </div>
                        <div class="col-2 col-sm-3 text-center align-self-center">
                            <button :disabled="alreadyOrdered"
                                    class="btn btn-sm btn-outline-danger btn-delete"
                                    type="button"
                                    @click="deleteItem(mealIndex)">
                                <em class="fas fa-trash-alt align-self-center"></em>
                            </button>
                        </div>
                    </div>
                    <hr class="dividerLineFat">
                </div>

                <div class="row">
                    <div class="col-sm-5"></div>
                    <div class="col-12 col-sm-5">
                        <div class="row mt-4">
                            <div class="col-9">
                                <h4 class="font-weight-bold">
                                    {{ __('shopping_item_totalAmountText') }}
                                </h4>
                                <p class="mwst font-weight-light">
                                    {{ __('shopping_infoMWST') }}
                                </p>
                            </div>
                            <div class="col-3">
                                <p id="orderTotalSum" class="font-weight-bold text-right">
                                    {{ completePrice }}
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-2"></div>
                </div>
                <div class="row mt-3">
                    <div class="col-12 col-sm-6">
                        <div class="row btn-sm-position">
                            <a :href="homeRoute" class="btn btn-info" type="button">
                                <em class="fas fa-chevron-left align-self-center"></em>
                                <span class="ml-2 align-self-center">
                                    {{ __('shopping_toMeals') }}
                                </span>
                            </a>
                        </div>
                    </div>
                    <div class="col-12 col-sm-6 text-right">
                        <div class="row btn-sm-position justify-content-end">
                            <button :disabled="alreadyOrdered"
                                    class="btn btn-info t"
                                    type="button"
                                    @click="sendOrder">
                                <span class="mr-2 align-self-center">
                                    {{ __('shopping_order') }}
                                </span>
                                <em class="fas fa-chevron-right align-self-center"></em>
                            </button>
                        </div>
                    </div>
                </div>
                <hr class="dividerLine">
                <div id="accordion">
                    <div class="card mt-2 mb-3">
                        <div class="card-header">
                            <a class="card-link" data-toggle="collapse" href="#collapseOne">
                                <p style="color: black">
                                    {{ __('shopping_older_orders') }}
                                </p>
                            </a>
                        </div>
                        <div id="collapseOne" class="collapse show" data-parent="#accordion">
                            <div class="card-body">
                                <div v-for="(oldOrder, oldOrderIndex) in orderOld">
                                    <div class="row">
                                        <div class="col-12">
                                            <p>
                                                <strong>
                                                    {{ __('shopping_one_order') }} {{ oldOrderIndex + 1 }}
                                                </strong>
                                            </p>
                                        </div>
                                    </div>
                                    <div v-for="(oldMeal, oldMealIndex) in orderMealItemsOld[oldOrderIndex]">
                                        <div v-for="oldSubMealIndex in oldMeal.amount">
                                            <div class="row">
                                                <div class="col-12">
                                                    <p>
                                                        {{ oldMeal.name }} #{{ oldSubMealIndex }}
                                                        {{
                                                            getOldIngredients(oldOrderIndex, oldMealIndex, oldSubMealIndex - 1)
                                                        }}
                                                    </p>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <p>
                                        <strong>
                                            {{ oldOrder.price }} €
                                        </strong>
                                    </p>
                                    <div v-if="orderStatus[oldOrderIndex] === 1 || orderStatus[oldOrderIndex] === 2 || orderStatus[oldOrderIndex] === 3">
                                        <div class="card m-sm-4">
                                            <div class="card-header">
                                                <p>
                                                    {{ __('shopping_order_status') }}
                                                </p>
                                            </div>
                                            <div class="card-body">
                                                <div class="row">
                                                    <div class="col-1"></div>
                                                    <div class="col-2 text-center align-self-center">
                                                        <div v-if="orderStatus[oldOrderIndex] === 1">
                                                            <em class="far fa-comment align-self-center m-2 orderIcons iconColorInProcess"></em>
                                                            <p class="iconColorInProcess">
                                                                {{ __('shopping_order') }}
                                                            </p>
                                                        </div>
                                                        <div
                                                            v-else-if="orderStatus[oldOrderIndex] === 2 || orderStatus[oldOrderIndex] === 3">
                                                            <em class="far fa-comment align-self-center m-2 orderIcons iconColorComplete"></em>
                                                            <p class="iconColorComplete">
                                                                {{ __('shopping_order') }}
                                                            </p>
                                                        </div>
                                                    </div>
                                                    <div class="col-2 align-self-center">
                                                        <div v-if="orderStatus[oldOrderIndex] === 1"
                                                             class="progress m-2">
                                                            <div class="progress-bar"></div>
                                                        </div>
                                                        <div
                                                            v-if="orderStatus[oldOrderIndex] === 2 || orderStatus[oldOrderIndex] === 3"
                                                            class="progress m-2">
                                                            <div class="progress-bar progress-completed"></div>
                                                        </div>
                                                    </div>
                                                    <div class="col-2 text-center align-self-center">
                                                        <div v-if="orderStatus[oldOrderIndex] === 1">
                                                            <em class="fas fa-utensils align-self-center m-2 orderIcons"></em>
                                                            <p>
                                                                {{ __('shopping_order_status_cooking') }}
                                                            </p>
                                                        </div>
                                                        <div v-else-if="orderStatus[oldOrderIndex] === 2">
                                                            <em class="fas fa-utensils align-self-center m-2 orderIcons iconColorInProcess"></em>
                                                            <p class="iconColorInProcess">
                                                                {{ __('shopping_order_status_cooking') }}
                                                            </p>
                                                        </div>
                                                        <div v-else-if="orderStatus[oldOrderIndex] === 3">
                                                            <em class="fas fa-utensils align-self-center m-2 orderIcons iconColorComplete"></em>
                                                            <p class="iconColorComplete">
                                                                {{ __('shopping_order_status_cooking') }}
                                                            </p>
                                                        </div>
                                                    </div>
                                                    <div class="col-2 align-self-center">
                                                        <div
                                                            v-if="orderStatus[oldOrderIndex] === 1 || orderStatus[oldOrderIndex] === 2"
                                                            class="progress m-2">
                                                            <div class="progress-bar"></div>
                                                        </div>
                                                        <div v-else-if="orderStatus[oldOrderIndex] === 3"
                                                             class="progress m-2">
                                                            <div class="progress-bar progress-completed"></div>
                                                        </div>
                                                    </div>
                                                    <div class="col-2 text-center align-self-center">
                                                        <div
                                                            v-if="orderStatus[oldOrderIndex] === 1 || orderStatus[oldOrderIndex] === 2">
                                                            <em class="far fa-check-circle align-self-center m-2 orderIcons"></em>
                                                            <p>
                                                                {{ __('shopping_order_status_eating') }}
                                                            </p>
                                                        </div>
                                                        <div v-else-if="orderStatus[oldOrderIndex] === 3">
                                                            <em class="far fa-check-circle align-self-center m-2 orderIcons iconColorInProcess"></em>
                                                            <p class="iconColorInProcess">
                                                                {{ __('shopping_order_status_eating') }}
                                                            </p>
                                                        </div>
                                                    </div>
                                                    <div class="col-1"></div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <hr class="dividerLine">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>


export default {
    name: 'shoppingCart',
    props: {
        orderData: Object,
        homeRoute: String
    },
    created() {
        console.log("route: ", this.homeRoute);
    },
    data() {
        return {
            message: 'Hello Vue.js!',
            order: {},
            itemCollapsed: [],
            itemPrices: [],
            ingredientAmounts: [],
            ingredientCheckBoxes: [],
            orderStatus: [],
            alreadyOrdered: false,
            orderMeal: {
                id: '',
                status: '',
                name: '',
                price: '',
                order_image: '',
                amount: '',
                order_description: '',
                active: '',
                order_id: '',
                created_at: '',
                updated_at: ''
            },
            orderOld: {},
            orderMealItemsOld: [],
            orderIngredientsOld: []
        }
    },
    mounted: function () {
        console.log("Meine Komponente läuft!");
        this.order = JSON.parse(JSON.stringify(this.orderData));
        console.log('order', this.order);

        this.orderOld = this.order[3];
        console.log("Alte Order: ", this.orderOld);
        for (let i = 0; i < this.order[4].length; i++) {
            this.orderMealItemsOld.push(this.order[4][i]);
        }
        console.log("Alte OrderMealItems: ", this.orderMealItemsOld);
        this.orderIngredientsOld = this.order[5];
        console.log("Alte Order Meal Ingredients: ", this.orderIngredientsOld);

        //initialize item price to 0.00
        for (var i = 0; i < this.order[0].length; i++) {
            this.itemPrices.push(parseFloat('0.0'));
        }

        //initialize ingredient amounts
        //iterate several items and pushing into itemArray
        for (let i = 0; i < this.order[6].length; i++) {
            //iterate their submeals and pushing into submealArray
            let submealArray = [];
            for (let j = 0; j < this.order[6][i].length; j++) {
                //iterate their ingredients and pushing into ingredientArray
                let ingredientArray = [];
                for (let k = 0; k < this.order[6][i][j].length; k++) {
                    ingredientArray.push(this.order[6][i][j][k].amount);
                }
                submealArray.push(ingredientArray);
            }
            this.ingredientAmounts.push(submealArray);
        }
        console.log('Initiale Ingredient Amounts der SubMeals: ', this.ingredientAmounts);

        //initialize ingredient checkBox values
        //iterate several items and pushing into ingredientCheckBoxes
        for (let i = 0; i < this.order[1].length; i++) {
            //iterate their submeals and pushing into ingredientCheckBoxSubmealArray
            let ingredientCheckBoxSubmealArray = [];
            for (let j = 0; j < this.order[1][i].length; j++) {
                //iterate their ingredients and pushing into ingredientCheckBoxIngredientArray
                let ingredientCheckBoxIngredientArray = [];
                for (let k = 0; k < this.order[1][i][j].length; k++) {
                    ingredientCheckBoxIngredientArray.push(1);
                }
                ingredientCheckBoxSubmealArray.push(ingredientCheckBoxIngredientArray);
            }
            this.ingredientCheckBoxes.push(ingredientCheckBoxSubmealArray);
        }
        console.log('Initiale Ingredient Checkboxes der SubMeals: ', this.ingredientCheckBoxes);

        for (let i = 0; i < this.order[0].length; i++) {
            this.itemPrice(i);
        }
        console.log('Inititale Item Preise: ', this.itemPrices);

        console.log("test: ", this.order);
        axios.get('/de/shoppingCart/getDeliveryStatus/' + this.order[3][0].user_id).then((response) => {
            let data = response.data;
            console.log("Ankommende initiale Stati: ", data);
            for (let i = 0; i < data.length; i++) {
                this.orderStatus[i] = data[i].status;
            }
            console.log("Eingetragene initiale Stati: ", this.orderStatus);
            this.$forceUpdate();
        });

        //order button enable/disable
        //ingredientCollapse initialize
        for (let i = 0; i < this.order[0].length; i++) {
            this.itemCollapsed.push(false);
        }

        //if user enters shoppingCart without any meal selected, the order button should be disabled
        if (this.order[0].length === 0) {
            this.alreadyOrdered = true;
        }

        console.log("Order Id: ", this.order[3][0].id);
        this.getDeliveryStatus(this.order[3][0].user_id);
    },
    computed: {
        completePrice: function () {
            var price = 0;
            for (let i = 0; i < this.itemPrices.length; i++) {
                price += parseFloat(parseFloat(this.itemPrices[i]).toFixed(2));
            }
            //sobald build abgeschlossen, ist order erreichbar
            if (this.order[2]) {
                this.order[2][0].price = price.toFixed(2);
            }
            return price.toFixed(2);
        }
    },
    methods: {
        itemPrice: function (itemId) {
            let price = 0;
            price += parseFloat((parseFloat(this.order[0][itemId].price) * this.order[0][itemId].amount).toFixed(2));

            //loop through submeals
            for (let i = 0; i < this.ingredientAmounts[itemId].length; i++) {
                //loop through ingredient amount of each submeal
                for (let j = 0; j < this.ingredientAmounts[itemId][i].length; j++) {
                    if (this.ingredientCheckBoxes[itemId][i][j] === 1) {
                        price += parseFloat((parseFloat(this.order[1][itemId][i][j].price) * this.ingredientAmounts[itemId][i][j]).toFixed(2));
                    }
                }
            }
            price = price.toFixed(2);
            console.log("Berechne ItemPrice: " + price);
            this.itemPrices[itemId] = price;
            console.log("Aktuelle ItemPrices: ", this.itemPrices);
        },
        addMeal: function (itemId) {
            this.order[0][itemId].amount = this.order[0][itemId].amount + 1;
            //add ingredients to ingredientAmounts and ingredientCheckBoxes
            let tmpIngredientAmounts = [];
            for (let i = 0; i < this.ingredientAmounts[itemId][0].length; i++) {
                tmpIngredientAmounts.push(1);
            }
            this.ingredientAmounts[itemId].push(tmpIngredientAmounts);
            let tmpCheckBoxes = [];
            for (let i = 0; i < this.ingredientCheckBoxes[itemId][0].length; i++) {
                tmpCheckBoxes.push(1);
            }
            this.ingredientCheckBoxes[itemId].push(tmpCheckBoxes);

            console.log('Aktuelle Ingredient Amounts der SubMeals: ', this.ingredientAmounts);
            console.log('Aktuelle Ingredient Checkboxes der SubMeals: ', this.ingredientCheckBoxes);

            //add ingredients to the order for reactive change of site
            this.order[1][itemId].push(this.order[1][itemId][0]);

            this.itemPrice(itemId);
        },
        deleteMeal: function (itemId) {
            if (this.order[0][itemId].amount > 1) {
                this.order[0][itemId].amount = this.order[0][itemId].amount - 1;
                //delete ingredients from ingredientAmounts and ingredientCheckBoxes
                this.ingredientAmounts[itemId].pop();
                this.ingredientCheckBoxes[itemId].pop();

                console.log('Aktuelle Ingredient Amounts der SubMeals: ', this.ingredientAmounts);
                console.log('Aktuelle Ingredient Checkboxes der SubMeals: ', this.ingredientCheckBoxes);

                //delete ingredients from the order for reactive change of site
                this.order[1][itemId].pop();

                this.itemPrice(itemId);
            }
        },
        incrementIngredientAmount: function (itemIndex, subMealIndex, ingredientIndex) {
            this.ingredientAmounts[itemIndex][subMealIndex][ingredientIndex] += 1;
            if (this.ingredientCheckBoxes[itemIndex][subMealIndex][ingredientIndex] === 1) {
                this.itemPrice(itemIndex);
            }
        },
        decrementIngredientAmount: function (itemIndex, subMealIndex, ingredientIndex) {
            if (this.ingredientAmounts[itemIndex][subMealIndex][ingredientIndex] > 1) {
                this.ingredientAmounts[itemIndex][subMealIndex][ingredientIndex] -= 1;
                if (this.ingredientCheckBoxes[itemIndex][subMealIndex][ingredientIndex] === 1) {
                    this.itemPrice(itemIndex);
                }
            }
        },
        checkIngredient: function (itemIndex, subMealIndex, ingredientIndex) {
            if (this.ingredientCheckBoxes[itemIndex][subMealIndex][ingredientIndex] == 0) {
                this.ingredientCheckBoxes[itemIndex][subMealIndex][ingredientIndex] = 1;
            } else {
                this.ingredientCheckBoxes[itemIndex][subMealIndex][ingredientIndex] = 0;
            }
            console.log(this.ingredientCheckBoxes);
            this.itemPrice(itemIndex);
        },
        deleteItem: function (itemIndex) {
            console.log("ID des geloeschten Items: ", this.order[0][0].id);
            let data;
            //if you delete last meal item
            if (this.order[0].length === 1) {
                //mealID and orderID
                data = [this.order[0][0].id, this.order[2][0].id];
            } else {
                data = [this.order[0][0].id, null];
            }
            console.log("Loeschen Data Array: ", data);
            axios.post('/de/shoppingCart/deleteItem', data);
            this.order[0].splice(itemIndex, 1);
            this.order[1].splice(itemIndex, 1);
            this.itemPrices.splice(itemIndex, 1);
            this.ingredientAmounts.splice(itemIndex, 1);
            this.ingredientCheckBoxes.splice(itemIndex, 1);
            console.log("Order nach dem Delete: ", this.order[0]);
            console.log("Item Prices nach dem Delete: ", this.itemPrices);
            console.log("Ingredient Amounts nach dem Delete: ", this.ingredientAmounts);
            console.log("Ingredient Check Boxes nach dem Delete: ", this.ingredientCheckBoxes);
        },
        getDeliveryStatus(userId) {
            setTimeout(() => {
                axios.get('/de/shoppingCart/getDeliveryStatus/' + userId)
                    .then((response) => {
                        console.log("Status UserId: ", userId);
                        let data = response.data;
                        console.log("Ankommende spaetere Stati: ", data);
                        for (let i = 0; i < data.length; i++) {
                            this.orderStatus[i] = data[i].status;
                        }
                        this.$forceUpdate();
                    })
                this.getDeliveryStatus(userId);
            }, 5000);
        },
        sendOrder: function () {
            if (this.order[0].length > 0){
                this.alreadyOrdered = true;
                let dataArr = [this.order, this.ingredientCheckBoxes, this.ingredientAmounts];
                console.log("Order welche zur DB geschickt wird: ", dataArr);
                axios.post('/de/shoppingCart/store', dataArr);

                //adding newly ordered order on top of the old orders list on site
                this.orderOld.push(this.order[2][0]);
                this.orderMealItemsOld.push(this.order[0]);
                this.orderIngredientsOld.push(this.order[1]);
                //and the orderStatus for newly ordered order
                this.orderStatus.push(1);

                //change order because push added items to list bottom
                //save last/new item
                let leng = this.orderOld.length;
                let lastOrderOld = this.orderOld[leng - 1];
                let lastOrderMealItemsOld = this.orderMealItemsOld[leng - 1];
                let lastOrderIngredientsOld = this.orderIngredientsOld[leng - 1];

                let lastOrderStatus = this.orderStatus[leng - 1];
                //pass old items to their next
                for (let i = leng - 1; i > 0; i--) {
                    this.orderOld[i] = this.orderOld[i - 1];
                    this.orderMealItemsOld[i] = this.orderMealItemsOld[i - 1];
                    this.orderIngredientsOld[i] = this.orderIngredientsOld[i - 1];

                    this.orderStatus[i] = this.orderStatus[i - 1];
                }
                //put last/new item at list top
                this.orderOld[0] = lastOrderOld;
                this.orderMealItemsOld[0] = lastOrderMealItemsOld;
                this.orderIngredientsOld[0] = lastOrderIngredientsOld;

                this.orderStatus[0] = lastOrderStatus;
            }
        },
        ingredientCollapse: function (itemIndex) {
            this.itemCollapsed[itemIndex] = (this.itemCollapsed[itemIndex]) ? false : true;
            console.log("itemCollapsed: ", this.itemCollapsed);
            this.$forceUpdate();
        },
        getOldIngredients: function (oldOrderIndex, oldMealIndex, oldSubMealIndex) {
            if (this.orderIngredientsOld[oldOrderIndex][oldMealIndex][oldSubMealIndex].length === 0) {
                return "";
            }
            let res = "(";
            for (let i = 0; i < this.orderIngredientsOld[oldOrderIndex][oldMealIndex][oldSubMealIndex].length; i++) {
                if (i === this.orderIngredientsOld[oldOrderIndex][oldMealIndex][oldSubMealIndex].length - 1) {
                    res += this.orderIngredientsOld[oldOrderIndex][oldMealIndex][oldSubMealIndex][i].name;
                } else {
                    res += this.orderIngredientsOld[oldOrderIndex][oldMealIndex][oldSubMealIndex][i].name + ', ';
                }
            }
            console.log("Namen der alten Ingredients: ", res);
            return res + ')';
        },

        __(key, replace) {
            console.log(window._translations);
            let translation, translationNotFound = true

            try {
                translation = key.split('.').reduce((t, i) => t[i] || null, window._translations[window._locale]['php'])

                if (translation) {
                    console.log("translation", " not", "found")
                    translationNotFound = false
                }
            } catch (e) {
                translation = key
            }

            if (translationNotFound) {
                translation = window._translations[window._locale]['json'][key]
                    ? window._translations[window._locale]['json'][key]
                    : key
            }

            _.forEach(replace, (value, innerKey) => {
                translation = translation.replace(':' + innerKey, value)
            })
            console.log(translation);
            return translation
        }
    }
}

</script>
