```javascript
function calculateDiscount(price) {
  if(!price) throw new Error('PriceIsRequiredParameter');
  if(!isNumber(price)) throw new Error('NotANumber');
  if(price < 0) throw new Error('NegativeNumber');

  if(price === 0) return 0;

  if(price >= 1000) return price * 0.25;

  if(price >= 500) return price * 0.1;

  if(price >= 200) return price * 0.05;

  return 0;
}
```