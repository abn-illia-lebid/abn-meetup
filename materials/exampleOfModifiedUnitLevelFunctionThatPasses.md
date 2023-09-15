```javascript
function validatePrice(price) {
	if(!price) throw new Error('PriceIsRequiredParameter');
  if(!isNumber(price)) throw new Error('NotANumber');
  if(price < 0) throw new Error('NegativeNumber');
}
 
function calculateDiscount(price) {
 	const error = validatePrice(price);

	if(error) throw error;

	if(price < 200) return 0;
	else if(price < 500) return price * 0.05;
	else if(price < 1000) return price * 0.1;
	else return price * 0.25;
}
```