Renders a placeholder element for the expiration date. The SqPaymentForm JS library will replace this
element with a secure `<iframe>` tag that will render an `<input>` field for the expiration date.

When accepting credit card payments, you **must** have this component inside your `SquarePaymentForm`.
## Props
|Name|Type|Description|
|---|---|---|
|label|string|Input field label<br/><br/>**Default Value:**`'Expiration'`|