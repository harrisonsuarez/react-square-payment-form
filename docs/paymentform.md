---
id: paymentform
title: Build the Payment Form
---

In this guide, we will build a simple payment form in Sandbox mode and generate a nonce that can be used to create a charge.

## 1. Create a Square developer account

To get started with the Square Payment Form, you will need to do the following:

1. [Create a Square Developer account](https://connect.squareup.com/apps).
2. [Create an application](https://connect.squareup.com/apps/new) and aggree to our [developer terms of service](https://squareup.com/us/en/legal/general/developers).

## 2. Get your Sandbox IDs

To get your Sandbox IDs:

1. Open your [Application Dashboard](https://connect.squareup.com/apps).
2. Click on the application you want to use for the payment form.
3. On the "Credentials" tab of the application control panel, copy the **Sandbox Application ID**.
4. On the "Locations" tab of the application control panel, copy one of the **Sandbox Location ID** values from the list of Sandbox locations.

The **application ID** tells us which developer is using the form to take payments. The **location ID** tells us which merchant location should recieve the payments.


## 3. Install the component

```
npm install --save react-square-payment-form
```

## 4. Import the component and styles

We provide default styles to make it easier to get set up.

```
import SquarePaymentForm from 'react-square-payment-form'
import 'react-square-payment-form/lib/default.css'
```

## 5. Add the SquarePaymentForm

The `SquarePaymentForm` is a wrapper that loads the SqPaymentForm JS library. It does not render anything on its own.

You will need to provide the `applicationId`, `locationId`, and `onCardNonceResponseRecieved`. `onCardNonceResponseRecieved` will return errors for invalid credit cards.

```
class PaymentPage extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      errorMessages: [],
    }
  }

  cardNonceResponseReceived = (errors, nonce, cardData) => {
    if (errors) {
      this.setState({ errorMessages: errors.map(error => error.message) })
      return
    }

    this.setState({ errorMessages: [] })
    alert("nonce created: " + nonce)
  }

  render() {
    return (
      <div>
        <h1>Payment Page</h1>

        <SquarePaymentForm
          applicationId={SANDBOX_APPLICATION_ID}
          locationId={SANDBOX_LOCATION_ID}
          cardNonceResponseReceived={this.cardNonceResponseReceived}
        >
        </SquarePaymentForm>

        <div className="sq-error-message">
          {this.state.errorMessages.map(errorMessage =>
            <li key={`sq-error-${errorMessage}`}>{errorMessage}</li>
          )}
        </div>

      </div>
    )
  }
}
```

## 6. Add the credit card fields

We will use a credit card form to test nonce creation. Digital wallets are not supported in Sandbox mode.

You must include all of the following components inside the `SquarePaymentForm`:
* `CreditCardNumberInput`
* `CreditCardExpirationDateInput`
* `CreditCardPostalCodeInput`
* `CreditCardCVVInput`
* `CreditCardSubmitButton`

These fields can be placed anywhere within the form and do not need to be in any specific order.
We have chosen to render the expiration date, postal code, and cvv on one line.

```
import SquarePaymentForm, {
  CreditCardNumberInput,
  CreditCardExpirationDateInput,
  CreditCardPostalCodeInput,
  CreditCardCVVInput,
  CreditCardSubmitButton
} from 'react-square-payment-form'

<SquarePaymentForm {...props}>

  <fieldset className="sq-fieldset">
    <CreditCardNumberInput />
    <div className="sq-form-third">
      <CreditCardExpirationDateInput />
    </div>

    <div className="sq-form-third">
      <CreditCardPostalCodeInput />
    </div>

    <div className="sq-form-third">
      <CreditCardCVVInput />
    </div>
  </fieldset>

  <CreditCardSubmitButton>
      Pay $1.00
  </CreditCardSubmitButton>

</SquarePaymentForm>
```

You should now see the credit card payment form.

![Credit Card Form](assets/credit-card-form.png)

## 7. Generate a nonce

We are ready to start creating nonces! Errors should render when you submit with an invalid credit card.

Fill in the form with the following test credit card information and click the "Pay" button. A nonce should be generated and an alert should appear on your browser.

* **Card Number**: 4532 7597 3454 5858
* **CVV**: any three non-consecutive numbers
* **Expiration Date**: any month and year in the future
* **Postal Code**: 94103
