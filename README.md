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
