# paypal-js


<a href="https://developer.paypal.com/docs/checkout/standard/customize/validate-user-input/">
    https://developer.paypal.com/docs/checkout/standard/customize/validate-user-input/
</a>



```js


        document.querySelector('#paypal-payment-button')
            .style.display = 'none';

        paypal.Buttons().render('#paypal-payment-button');

        document.querySelector('#myRadioField')

        .addEventListener('click', function() {

            if (document.getElementById('billing_first_name').value === "") {
                alert("Please enter your first name.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            document.querySelector('#paypal-payment-button')
                .style.display = 'block';
        });


```


```js



<script src="https://www.paypal.com/sdk/js?client-id=AVjwkp_lbSvunVpU8EceeP8ltOzEpvfbvj_GL-yL3Zp4dazgXUp9We6Oy8U6gaoVoSgSbyTS4H3UbImK&disable-funding=credit,card"></script>
<script>

    function paypal_button()
    {
        console.log('wowsls');
    }


    paypal.Buttons({
        style : {
            color: 'blue',
            shape: 'pill'
        },


        //onInit is called when the button first renders

        onClick: function(data, actions) {

            // You must return a promise from onClick to do async validation
            if (document.getElementById('price_lkr').value === "") {
                alert("Please enter your address.");
                actions.disable();
            }
            
            
        },

        createOrder: function (data, actions) {
            return actions.order.create({
                purchase_units : [{
                    amount: {
                        value: '23'
                    },
                }]
            });
        },
        onApprove: function (data, actions) {
            return actions.order.capture().then(function (details) {
                console.log(details)
                window.location.href = '{{route('paypal.success')}}'
            })
        },
        onCancel: function (data) {
            window.location.href = '{{route('paypal.cancel')}}'
        }
    }).render('#paypal-payment-button');

</script>

```

<p> example code </p>

```
 @extends('layouts.front')



@section('content')


    <div class="container" style="margin-top: 5em;margin-bottom: 5em;">
        <div class="row">
            <div class="col-lg-7">
                <label for="">Select Payment Method</label>
                <select class="form-control" id="select1" onchange="setForm(this.value)">
                    <option value="form1">Online Payment</option>
                    <option value="form2">Cash on Delivery</option>
                </select>
            </div>
        </div>
    </div>



<div class="checkout-area mt-60px mb-40px">


<div id="form1">
        <div class="container">
            <div class="row">
                <div class="col-lg-7">
                    <label for="">Online Payment</label>
                    <div class="billing-info-wrap">
                     <h3>Delivery Details</h3>
                        <fieldset id="billingAddress" class="text">
                            <div class="row">
                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>First Name</label>
                                        <input type="text" class="form-control" id="op_billing_first_name" />
                                    </div>
                                </div>
                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Last Name</label>
                                        <input type="text" class="form-control" id="op_billing_last_name" />
                                    </div>
                                </div>

                                <div class="col-lg-12">
                                    <div class="billing-info mb-5">
                                        <label>Street Address</label>
                                        <input  class="form-control" id="op_billing_address" placeholder="House number and street name" type="text" />
                                    </div>
                                </div>


                                <div class="col-lg-12">
                                    <div class="billing-info mb-5">
                                        <label>Town / City</label>
                                        <input type="text" class="form-control" id="op_billing_town" />
                                    </div>
                                </div>


                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Email Address</label>
                                        <input type="email" class="form-control" id="op_billing_email" placeholder="Enter email"  />
                                    </div>
                                </div>

                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Phone</label>
                                        <input type="text" class="form-control" id="op_billing_phone" />
                                    </div>
                                </div>

                            </div>
                        </fieldset>



                        <div class="additional-info-wrap">
                            <h4>Additional information</h4>
                            <div class="additional-info mb-5">
                                <label>Order notes</label>
                                <textarea placeholder="Notes about your order, e.g. special notes for delivery. "  class="form-control" name="message" id="op_delivery_notes" maxlength="255"></textarea>
                            </div>
                        </div>



                        <div class="different-address mt-30">


                    </div>



                    </div>
                </div>



                <div class="col-lg-5">
                    <div class="your-order-area">
                        <h3>Your order (Online Payment)</h3>
                        <div class="your-order-wrap gray-bg-4">
                            <div class="your-order-product-info">

                                <div class="your-order-middle">


                                    @if(Cart::count()>0)


                                        @foreach(Cart::content() as $item)
                                          <p><span class="order-middle-left"> <strong>{{$item->name}}  </strong>X ({{$item->qty}}) </span> <span class="order-price"> {{$item->price}} </span></p>
                                        @endforeach


                                    @endif

                                </div>
                                <div class="your-order-total">
                                        <p class="order-total"><strong>Sub Total :</strong> {{Cart::subtotal()}}</p>
                                    <input type="hidden" value="{{Cart::subtotal(0,'','')}}" id="value_total">
                                </div>
                            </div>

                            <div class="payment-method">
                                <div class="payment-accordion element-mrg">
                                    <div class="panel-group" id="accordion">
                                        <div class="panel payment-accordion">
                                            <div class="panel-heading" id="method-one">
                                            </div>
                                            <div id="method1" class="panel-collapse collapse show">
                                                <div class="panel-body">
                                                    <p>Please send a check to Store Name, Store Street, Store Town, Store State / County, Store Postcode.</p>
                                                    <lable>
                                                        I Agree to the term and condition
                                                    </lable>
                                                    <input type="checkbox" id="myRadioField" name="fav_language" value="HTML">
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        @if(Cart::count()>0)
                        <div class="Place-order mt-25">
                            <br>
                            <div id="paypal-payment-button">
                            </div>
                            <img src="{{asset('ecom_assets/croped-paypal-img.png')}}" style="height: auto;width: 100%;" onclick="cod_place_order_Function()" id="paypalLogo" alt="">
                        </div>
                        @endif
                    </div>
                </div>
            </div>
        </div>

</div>


    <div  id="form2" style="display: none">


        <div class="container">
            <div class="row">
                <div class="col-lg-7">
                    <label for="">Cash on delivery</label>
                    <div class="billing-info-wrap">
                        <h3>Delivery Details</h3>
                        <fieldset id="billingAddress" class="text">
                            <div class="row">
                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>First Name</label>
                                        <input type="text" class="form-control" id="cod_billing_first_name" />
                                    </div>
                                </div>
                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Last Name</label>
                                        <input type="text" class="form-control" id="cod_billing_last_name" />
                                    </div>
                                </div>

                                <div class="col-lg-12">
                                    <div class="billing-info mb-5">
                                        <label>Street Address</label>
                                        <input  class="form-control" id="cod_billing_address" placeholder="House number and street name" type="text" />
                                    </div>
                                </div>


                                <div class="col-lg-12">
                                    <div class="billing-info mb-5">
                                        <label>Town / City</label>
                                        <input type="text" class="form-control" id="cod_billing_town" />
                                    </div>
                                </div>


                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Email Address</label>
                                        <input type="email" class="form-control" id="cod_billing_email" placeholder="Enter email"  />
                                    </div>
                                </div>

                                <div class="col-lg-6 col-md-6">
                                    <div class="billing-info mb-5">
                                        <label>Phone</label>
                                        <input type="text" class="form-control" id="cod_billing_phone" />
                                    </div>
                                </div>

                            </div>
                        </fieldset>



                        <div class="additional-info-wrap">
                            <h4>Additional information</h4>
                            <div class="additional-info mb-5">
                                <label>Order notes</label>
                                <textarea placeholder="Notes about your order, e.g. special notes for delivery. "  class="form-control" name="message" id="cod_delivery_notes" maxlength="255"></textarea>
                            </div>
                        </div>



                        <div class="different-address mt-30">


                        </div>



                    </div>
                </div>

                @php
                    $items = substr(base_convert(sha1(uniqid(mt_rand())), 16, 36), 0, 9);
                @endphp
                <input type="hidden" id="cod_order_id" name="cod_order_id" value="ItemNo{{$items}}">


                <div class="col-lg-5">
                    <div class="your-order-area">
                        <h3>Your order (Cash on Delivery)</h3>
                        <div class="your-order-wrap gray-bg-4">
                            <div class="your-order-product-info">

                                <div class="your-order-middle">


                                    @if(Cart::count()>0)


                                            @foreach(Cart::content() as $item)
                                                <p><span class="order-middle-left">{{$item->name}} X ({{$item->qty}}) </span> <span class="order-price"> {{$item->price}} </span></p>
                                            @endforeach


                                    @endif

                                </div>
                                <div class="your-order-total">

                                        <p class="order-total">Sub Total: {{Cart::subtotal()}}</p>
                                    <input type="hidden" value="{{Cart::subtotal(0,'','')}}" id="cod_value_total">
                                </div>
                            </div>

                            <div class="payment-method">
                                <div class="payment-accordion element-mrg">
                                    <div class="panel-group" id="accordion">
                                        <div class="panel payment-accordion">
                                            <div class="panel-heading" id="method-one">
                                            </div>
                                            <div id="method1" class="panel-collapse collapse show">
                                                <div class="panel-body">
                                                    <p>Please send a check to Store Name, Store Street, Store Town, Store State / County, Store Postcode.</p>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        @if(Cart::count()>0)
                        <div class="Place-order mt-25">
                            <br>
                           <button class="btn btn-primary" id="cash_on_delivery">Place Order</button>
                        </div>
                       @endif
                    </div>
                </div>
            </div>
        </div>

    </div>



</div>
@endsection




@section('extra-js')




    <script src="https://www.paypal.com/sdk/js?client-id=ATqJoT8uledW83BN2RvdA4o9tptMnGw4EUVlV1na6YHhKgqXEHcJXE8t0EZLGsDr4mybfMJ5nXxL10vQ"></script>




    <script type="text/javascript">
        function setForm(value) {

            if(value == 'form1'){
                document.getElementById('form1').style='display:block;';
                document.getElementById('form2').style='display:none;';
            }
            else {

                document.getElementById('form2').style = 'display:block;';
                document.getElementById('form1').style = 'display:none;';
            }

        }

    </script>



    <script>

        document.querySelector('#paypal-payment-button')
            .style.display = 'none';

        document.querySelector('#paypalLogo')
            .style.display = 'block';

        /*validation*/

        function cod_place_order_Function() {

            if (document.getElementById('op_billing_first_name').value === "") {
                alert("Please enter your first name.");
                return false;
            }

            if (document.getElementById('op_billing_last_name').value === "") {
                alert("Please enter your last name.");
                return false;
            }

            if (document.getElementById('op_billing_address').value === "") {
                alert("Please enter your billing address.");
                return false;
            }

            if (document.getElementById('op_billing_town').value === "") {
                alert("Please enter your billing town.");
                return false;
            }

            if (document.getElementById('op_billing_email').value === "") {
                alert("Please enter your billing email.");
                return false;
            }

            if (document.getElementById('op_billing_phone').value === "") {
                alert("Please enter your billing phone.");
                return false;
            }

            if (document.getElementById('op_delivery_notes').value === "") {
                alert("Please enter your delivery notes.");
                return false;
            }

            alert("Please select A Terms and Conditions agreement check box");

        }


        /*paypal button*/
        paypal.Buttons({
            createOrder: function (data, actions) {
                return actions.order.create({
                    purchase_units : [{
                        amount: {
                            value: '0.1'
                        },
                    }]
                });
            },
            onApprove: function (data, actions) {
                return actions.order.capture().then(function (details) {

                    var order_id = details.id;

                    /*post request*/

                    $.ajax({
                        type: "POST",
                        url: "/checkout/success",
                        data: {
                            "Billingfname": document.getElementById('op_billing_first_name').value,
                            "Billinglname": document.getElementById('op_billing_last_name').value,
                            "Billingaddress": document.getElementById('op_billing_address').value,
                            "Billingtown": document.getElementById('op_billing_town').value,
                            "Billingemail": document.getElementById('op_billing_email').value,
                            "Billingphone": document.getElementById('op_billing_phone').value,
                            "Deliverynotes": document.getElementById('op_delivery_notes').value,
                            "order_id" : order_id,
                            "_token": "{{ csrf_token() }}",
                        },
                        dataType: 'json',

                        /*------------------------*/

                        success: function (data) {
                             window.location.href = '{{route('thankyou')}}'
                        },
                        error: function (data) {
                            console.log('Error:', data.responseJSON);
                        }
                    });

                    /*post request*/



                })
            },
            onCancel: function (data) {
                window.location.replace("http://localhost/paypal-js/Oncancel.php")
            }
        }).render('#paypal-payment-button');


        document.querySelector('#myRadioField').addEventListener('click', function() {

            if (document.getElementById('op_billing_first_name').value === "") {
                alert("Please enter your first name.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_billing_last_name').value === "") {
                alert("Please enter your last name.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_billing_address').value === "") {
                alert("Please enter your billing address.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_billing_town').value === "") {
                alert("Please enter your billing town.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_billing_email').value === "") {
                alert("Please enter your billing email.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_billing_phone').value === "") {
                alert("Please enter your billing phone.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }

            if (document.getElementById('op_delivery_notes').value === "") {
                alert("Please enter your delivery notes.");
                document.getElementById("myRadioField").checked = false;
                return false;
            }


            document.querySelector('#paypalLogo')
                .style.display = 'none';

            document.querySelector('#paypal-payment-button')
                .style.display = 'block';

        });

    </script>

    <script>


        document.getElementById('cash_on_delivery').onclick = function (e) {

            e.preventDefault();


            if (document.getElementById('cod_billing_first_name').value === "") {
                alert("Please enter your first name.");
                return false;
            }

            if (document.getElementById('cod_billing_last_name').value === "") {
                alert("Please enter your last name.");
                return false;
            }

            if (document.getElementById('cod_billing_address').value === "") {
                alert("Please enter your address.");
                return false;
            }

            if (document.getElementById('cod_billing_town').value === "") {
                alert("Please enter your town.");
                return false;
            }

            if (document.getElementById('cod_billing_email').value === "") {
                alert("Please enter your email.");
                return false;
            }


            if (document.getElementById('cod_billing_phone').value === "") {
                alert("Please enter your phone number.");
                return false;
            }


            if (document.getElementById('cod_delivery_notes').value === "") {
                alert("Please enter delivery notes.");
                return false;
            }

            var cod_billing_first_name = document.getElementById('cod_billing_first_name').value;
            var cod_billing_last_name = document.getElementById('cod_billing_last_name').value;
            var cod_billing_address = document.getElementById('cod_billing_address').value;
            var cod_billing_town = document.getElementById('cod_billing_town').value;
            var cod_billing_email = document.getElementById('cod_billing_email').value;
            var cod_billing_phone = document.getElementById('cod_billing_phone').value;
            var cod_billing_notes = document.getElementById('cod_delivery_notes').value;



            var cod_value_total = document.getElementById('cod_value_total').value;
            var cod_order_id= document.getElementById('cod_order_id').value;

            $.ajax({
                type: "POST",
                url: "/cart/cash-one-delivery",
                data: {
                    "Billingfname": cod_billing_first_name,
                    "Billinglname": cod_billing_last_name,
                    "Billingaddress": cod_billing_address,
                    "Billingtown": cod_billing_town,
                    "Billingemail": cod_billing_email,
                    "Billingphone": cod_billing_phone,
                    "Deliverynotes": cod_billing_notes,
                    "order_id": cod_order_id,
                    "total": cod_value_total,
                    "_token": "{{ csrf_token() }}",
                },
                dataType: 'json',
                success: function (data) {
                    console.log(data);
                    window.location.href = '{{route('thankyou')}}'
                },
                error: function (data) {
                    console.log('Error:', data.responseJSON);
                }
            });


        }


    </script>



@endsection

```




