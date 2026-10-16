# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



## Example — delete this

### CC-99 — "The cart total is wrong"

**Reproduced:** Added 2 dosas at Rs. 60 each. The cart showed
Rs. 119.99999 instead of Rs. 130. Happened every time, on any dish with
a price ending in .50.

**Cause:** The total was being added up with plain floating point and
never rounded, so 0.1 + 0.2 style errors showed up on screen. The
rounding helper existed but this one place was not using it.

**Fix:** Ran the total through the existing rounding helper instead of
adding a new one, so every price on screen goes through the same path.

**Checked:** Cart, checkout and the order screen all show Rs. 130 now.
Prices without decimals still show without a trailing .00.

**Time:** about 40 minutes, most of it working out that the cart and the
order screen round in different places.



## CC-01 — "<Suggestions were appearing behind the cat-tabs>"

**Reproduced:** Searched for Pizza in search bar , suggestion box was appearing behind the cat-tabs and hence first suggestion was not visible  

**Cause:**Z-index was more than that of cat-tabs , Hence It was appearing above suggestions

**Fix:**Changed the value of Z-index of cat-tabs

**Checked:**Suggestions are appearing above cat-tabs now 

**Time:**about 30 minutes , had to figure out z-index property

## CC-02 — "Can't read anything in dark mode"

**Reproduced:** Opened the site and the dish names and prices were barely visible in dark mode , the text was blending into the dark background

**Cause:** The dish name button and price text were using `color: inherit` which wasn't being overridden for dark mode , so they stayed dark-colored on a dark background

**Fix:** Created a new CSS variable `--name-btn` with proper colors for both light (`#201913`) and dark (`#E5E7EB`) themes , applied it to `.dish-price`, the dish name button, and `.qty-value` so everything is readable in both modes

**Checked:** Switched between light and dark mode , dish names, prices, and quantity values are all clearly visible now in both themes

**Time:** about 45 minutes , had to trace which elements were affected and figure out the right approach with CSS variables

## CC-06 — "I ordered more than they had"

**Reproduced:** A dish had only 2 left in stock but the counter let me add 5 to the cart and the order went through

**Cause:** `setQty()` in `state.js` was only checking against `MAX_PER_DISH` but never checking the actual stock of the dish , so it would happily let you order more than what was available

**Fix:** Added a stock check in `setQty()` — it now looks up the dish's stock from `state.dishes` and returns an error with a message like "Only 2 left in the counter" if you try to add more than what's in stock

**Checked:** Tried ordering more than stock allows , it blocks the order and shows the right message now

**Time:** about an hour , had to understand how state.js worked and how dish data flows through the app

## CC-07 — "Cancelling makes it worse"

**Reproduced:** Cancelled an order and the stock went down again instead of coming back up

**Cause:** In `releaseStock()` in `validation.js` , it was doing `stock - line.qty` instead of `stock + line.qty` , so cancelling an order was subtracting stock again instead of adding it back

**Fix:** Changed the minus to a plus in `releaseStock()` — `dish.stock - line.qty` → `dish.stock + line.qty`

**Checked:** Placed an order then cancelled it , stock goes back up correctly now

**Time:** about 15-20 minutes , the bug was pretty obvious once I found the function

## CC-08 — "An old coupon still works"

**Reproduced:** Used the expired coupon FRESHERS24 and it still gave the discount even though it expired ages ago

**Cause:** `applyCoupon()` in `pricing.js` was checking if the coupon exists and if it had uses left , but it never checked the expiry date at all

**Fix:** Added an expiry check — if `new Date(coupon.expiresAt) < now` it returns "This coupon has expired" , also made sure the `expiresAt` field is sent to the frontend in the coupons route

**Checked:** Tried FRESHERS24 and it now gets rejected with the expired message , valid coupons still work fine

**Time:** about 30 minutes , had to check both the backend logic and the route to make sure expiry data was being passed through

## CC-10 — "Sorting by price is backwards"

**Reproduced:** Picked "price: low to high" and the most expensive items came first , "high to low" showed cheap ones first , both were swapped

**Cause:** The sort comparators in `search.js` were swapped — `price-asc` was doing `b.price - a.price` (which is descending) and `price-desc` was doing `a.price - b.price` (which is ascending)

**Fix:** Swapped them — `price-asc` now does `a.price - b.price` and `price-desc` does `b.price - a.price`

**Checked:** Both sort options work correctly now , low to high shows cheapest first and high to low shows most expensive first

**Time:** about 30 minutes , had to understand the sort system and verify the comparator logic

## CC-03 — "The menu is wider than my phone"

**Reproduced:** On a phone screen the menu was wider than the viewport , had to scroll sideways and Add to Cart buttons on the right side were cut off

**Cause:** `.dish-card` had `overflow: clip` which was preventing the cards from adapting properly to smaller screens

**Fix:** Changed `overflow: clip` to `overflow: auto` on `.dish-card` in `style.css`

**Checked:** Menu fits within the phone screen now , no sideways scrolling and all buttons are accessible

**Time:** about an hour , had to test on different screen sizes and figure out which CSS property was causing the overflow

## CC-04 — "The buttons don't work on my tablet"

**Reproduced:** On a tablet (761px-900px width) , Add to Cart and the star button did nothing , they looked normal but clicking had no effect , worked fine on laptop and phone

**Cause:** In the responsive CSS for `@media (min-width: 761px) and (max-width: 900px)` , there were `::after` pseudo-elements on `.dish-card` and `.img-wrap` with `content: ""` that created invisible overlay layers , these were sitting on top of the buttons and blocking all clicks

**Fix:** Removed the `content: ""` line from the `::after` pseudo-elements in that media query , so the invisible overlays don't get created anymore

**Checked:** Tested at tablet widths , Add to Cart and star buttons work properly now , no invisible blockers

**Time:** about an hour , this one was tricky because the buttons looked completely normal and I had to figure out that invisible pseudo-elements were blocking clicks



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.
