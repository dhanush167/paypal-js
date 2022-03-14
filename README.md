# paypal-js

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
