---


---

<h1 id="promo-engine-arch-overview">Promo Engine Arch Overview</h1>
<h2 id="background">Background</h2>
<p>Examples of promotions:</p>
<ul>
<li>Amount of a product</li>
<li>Percent of whole order</li>
<li>Free shipping for a product</li>
<li>Buy one, get one free</li>
<li>Free gift with purchase</li>
</ul>
<h3 id="urbncat-properties">URBNCat Properties</h3>

<table>
<thead>
<tr>
<th>Property Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>allowMultiple</td>
<td>Boolean value which determines if this promotion can be applied to the same customer multiple times.</td>
</tr>
<tr>
<td>beginUsable</td>
<td>Defines when the promotion becomes live</td>
</tr>
<tr>
<td>creationDate</td>
<td>Defines when the promotion was created</td>
</tr>
<tr>
<td>description</td>
<td>A short description</td>
</tr>
<tr>
<td>displayName</td>
<td>Specifies the name visible in the user interface.</td>
</tr>
<tr>
<td>enabled</td>
<td>Boolean value determining if the promotion can be used or not</td>
</tr>
<tr>
<td>endUsable</td>
<td>The date that the promotion stops being effective.</td>
</tr>
<tr>
<td>global</td>
<td>Automatically apply to all orders, ignores properties of: allowMultiple, startDate, endDate, giveToAnonymousProfiles, relativeExpiration, timeUntilExpire, uses</td>
</tr>
<tr>
<td>pmdlRule</td>
<td>The PMDL field (More details given later)</td>
</tr>
<tr>
<td>priority</td>
<td>Promotion ordering within Discount type (item, shipping, order) with lowest numbers having the highest priority</td>
</tr>
<tr>
<td>giveToAnonymousProfiles</td>
<td>Defines if a user must be logged in or registered to receive the promotion</td>
</tr>
<tr>
<td>uses</td>
<td>The number of orders that single customer can apply this promotion to (e.i. if you want to use this promotion more than the number of uses defined - you need to create a new account )</td>
</tr>
<tr>
<td>sites</td>
<td>(called siteIds on URBNCAT…) The siteConfiguration item or items with which the promotion is associated.</td>
</tr>
<tr>
<td>type</td>
<td><code>ITEM_DISCOUNT</code>, <code>SHIPPING_DISCOUNT</code>, or <code>ORDER_DISCOUNT</code></td>
</tr>
</tbody>
</table><h3 id="alternatively-available-properties">Alternatively Available properties</h3>
<p>Although URBNCat is not currently using these fields, they are important to note - as some of them are being used in the BCC by merchandisers.</p>

<table>
<thead>
<tr>
<th>Property Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>endDate</td>
<td>Used for promotion distribution</td>
</tr>
<tr>
<td>startDate</td>
<td>Used for promotion distribution</td>
</tr>
<tr>
<td>media</td>
<td>Media that is associated with this promotion, icons/images/ect</td>
</tr>
<tr>
<td>oneUsePerOrder</td>
<td>Allows a shipping discount to only be applied to a single shipping group within an order</td>
</tr>
<tr>
<td>relativeExpiration</td>
<td>Used in conjunction with the <code>timeUntilExpire</code> to keep a promotion live only for as long as the user has had the promotion</td>
</tr>
<tr>
<td>timeUntilExpire</td>
<td>Used in conjunction with the <code>relativeExpiration</code> to keep a promotion live only for as long as the user has had the promotion</td>
</tr>
</tbody>
</table><h2 id="urbncat">URBNCat</h2>
<ul>
<li>URBNCat will need to publish a new promotion field, which for now (during swapout period) will need to be a secondary process which is run for Promotion Publishing</li>
<li>This process will need to publish to a Queue which will be picked up by a CEP task</li>
<li>Promotions will now be published with the same document as they have been previously, but with the new promotion JSON Format included:</li>
</ul>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
   <span class="token string">"priority"</span><span class="token punctuation">:</span><span class="token number">1</span><span class="token punctuation">,</span>
   <span class="token string">"id"</span><span class="token punctuation">:</span><span class="token string">"000a4e94a0804d5c97f9a2479e7b25f9"</span><span class="token punctuation">,</span>
   <span class="token string">"endUsable"</span><span class="token punctuation">:</span><span class="token keyword">null</span><span class="token punctuation">,</span>
   <span class="token string">"uses"</span><span class="token punctuation">:</span><span class="token number">99999</span><span class="token punctuation">,</span>
   <span class="token string">"coupons"</span><span class="token punctuation">:</span><span class="token punctuation">[</span>
      <span class="token punctuation">{</span>
         <span class="token string">"internalUseOnly"</span><span class="token punctuation">:</span><span class="token boolean">false</span><span class="token punctuation">,</span>
         <span class="token string">"name"</span><span class="token punctuation">:</span><span class="token string">"coupon2"</span><span class="token punctuation">,</span>
         <span class="token string">"promotionIds"</span><span class="token punctuation">:</span><span class="token punctuation">[</span>
            <span class="token string">"000a4e94a0804d5c97f9a2479e7b25f9"</span><span class="token punctuation">,</span>
            <span class="token string">"d439f35de21b4e17b5aa810af86e4748"</span>
         <span class="token punctuation">]</span><span class="token punctuation">,</span>
         <span class="token string">"couponCode"</span><span class="token punctuation">:</span><span class="token string">"COUPON2"</span><span class="token punctuation">,</span>
         <span class="token string">"maxUses"</span><span class="token punctuation">:</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">,</span>
         <span class="token string">"endDate"</span><span class="token punctuation">:</span><span class="token keyword">null</span><span class="token punctuation">,</span>
         <span class="token string">"startDate"</span><span class="token punctuation">:</span><span class="token number">1516375200000</span>
      <span class="token punctuation">}</span>
   <span class="token punctuation">]</span><span class="token punctuation">,</span>
   <span class="token string">"promoRule"</span><span class="token punctuation">:</span> <span class="token string">"// Promotion JSON goes here"</span><span class="token punctuation">,</span>
   <span class="token string">"giveToAnonymousProfiles"</span><span class="token punctuation">:</span><span class="token boolean">true</span><span class="token punctuation">,</span>
   <span class="token string">"displayName"</span><span class="token punctuation">:</span><span class="token string">"1.61 ApplePay Promotion"</span><span class="token punctuation">,</span>
   <span class="token string">"global"</span><span class="token punctuation">:</span><span class="token boolean">false</span><span class="token punctuation">,</span>
   <span class="token string">"type"</span><span class="token punctuation">:</span><span class="token string">"ORDER_DISCOUNT"</span><span class="token punctuation">,</span>
   <span class="token string">"promotionGroup"</span><span class="token punctuation">:</span><span class="token keyword">null</span><span class="token punctuation">,</span>
   <span class="token string">"excludedGroups"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span>
   <span class="token string">"enabled"</span><span class="token punctuation">:</span><span class="token boolean">true</span><span class="token punctuation">,</span>
   <span class="token string">"creationDate"</span><span class="token punctuation">:</span><span class="token number">1516375206000</span><span class="token punctuation">,</span>
   <span class="token string">"beginUsable"</span><span class="token punctuation">:</span><span class="token number">1516375020000</span><span class="token punctuation">,</span>
   <span class="token string">"description"</span><span class="token punctuation">:</span><span class="token string">"1.61 ApplePay Promotion"</span><span class="token punctuation">,</span>
   <span class="token string">"siteIds"</span><span class="token punctuation">:</span><span class="token punctuation">[</span>
      <span class="token string">"uo-ca"</span><span class="token punctuation">,</span>
      <span class="token string">"uo-us"</span>
   <span class="token punctuation">]</span><span class="token punctuation">,</span>
   <span class="token string">"allowMultiple"</span><span class="token punctuation">:</span><span class="token boolean">true</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>The new Promotion Rule can be generated using the recursive <code>PromotionUtils.condition_group_to_json</code> function which already exists in URBNCat.</li>
<li>For the time being, this will be the only URBNCat change</li>
<li>For the time being, the existing Java Data types which are exported as part of the PMDL can be included in the PMDL</li>
</ul>
<p>An Example of the existing PMDL Can be seen below:</p>
<pre><code>&lt;pricing-model&gt;
	&lt;qualifier&gt;
		&lt;all&gt;
			&lt;collection-name&gt;items&lt;/collection-name&gt;
			&lt;element-name&gt;item&lt;/element-name&gt;
			&lt;and&gt;
				&lt;equals&gt;
					&lt;value&gt;item.priceInfo.currencyCode&lt;/value&gt;
					&lt;constant&gt;
						&lt;data-type&gt;java.lang.String&lt;/data-type&gt;
						&lt;string-value&gt;CAD&lt;/string-value&gt;
					&lt;/constant&gt;
				&lt;/equals&gt;
				&lt;equals&gt;
					&lt;value&gt;item.priceInfo.currencyCode&lt;/value&gt;
					&lt;constant&gt;
						&lt;data-type&gt;java.lang.String&lt;/data-type&gt;
						&lt;string-value&gt;CAD&lt;/string-value&gt;
					&lt;/constant&gt;
				&lt;/equals&gt;
				&lt;not&gt;
					&lt;equals&gt;
						&lt;value&gt;item.priceInfo.currencyCode&lt;/value&gt;
						&lt;constant&gt;
							&lt;data-type&gt;java.lang.String&lt;/data-type&gt;
							&lt;string-value&gt;CAD&lt;/string-value&gt;
						&lt;/constant&gt;
					&lt;/equals&gt;
				&lt;/not&gt;
				&lt;and&gt;
					&lt;and&gt;
						&lt;isoneof&gt;
							&lt;value&gt;order.originOfOrder&lt;/value&gt;
							&lt;constant&gt;
								&lt;data-type&gt;java.util.Collection&lt;/data-type&gt;
								&lt;string-value&gt;ios&lt;/string-value&gt;
								&lt;string-value&gt;android&lt;/string-value&gt;
							&lt;/constant&gt;
						&lt;/isoneof&gt;
					&lt;/and&gt;
				&lt;/and&gt;
			&lt;/and&gt;
		&lt;/all&gt;
	&lt;/qualifier&gt;
	&lt;offer&gt;
		&lt;discount-structure adjuster=\"100\" calculator-type=\"standard\" discount-type=\"amountOff\"/&gt;
	&lt;/offer&gt;
&lt;/pricing-model&gt;
</code></pre>
<ul>
<li>The associated JSON model will be broken apart by Qualifier/Quantifier/Offer, which (along with appropriate meta information) will need to be exported as part of the Document posted to the queue.</li>
<li>SiteGroup will need to be added into the JSON Document before it is posted to the Queue</li>
<li>Active will need to be added into the JSON Document before it is posted to the Queue</li>
</ul>
<h3 id="urbncat-future-changes">URBNCat Future Changes:</h3>
<ul>
<li>Some additional logic exists in the promotion publishing during PMDL Generation
<ul>
<li>Appending Channels</li>
<li>Appending <code>is-disountable</code> logic</li>
<li>Adding Tiered Discount XML to PMDL</li>
</ul>
</li>
</ul>
<h2 id="cep">CEP</h2>
<ul>
<li>A new Queue will need to be created as <code>A15-[ENV]-PROMO-JSON</code></li>
<li>A new task will need to be added which reads off of the newly created <code>A15-[ENV]-PROMO-JSON</code> Queue</li>
<li>This task will write the newly found promotion into the existing mongo instance, under a new collection
<ul>
<li><code>CatalogService.promotions</code></li>
<li>Indexed on <code>SiteGroup</code>, <code>Available</code>, <code>Coupons</code>, <code>BeginUsable</code>, and <code>EndUsable</code> using a <a href="https://docs.mongodb.com/manual/core/index-compound/">compound index</a>
<ul>
<li>This will allow sorting/filtering based on Begin/End date so that non-active promotions can be skipped</li>
</ul>
</li>
</ul>
</li>
<li>Create a new clean-up task which clears out promotions which are no longer active due to their <code>EndUsable</code> date having past (for more than 30 days)</li>
</ul>
<h2 id="promotion-engine">Promotion Engine</h2>
<ul>
<li>The promotion engine will need to be created as a python Class - which can be invoked from the Pricing Model</li>
<li>IF Bala disagrees with this, then the promo engine will need to have a wrapper created, which can read messages sent over some form of interprocess communication (HTTP/Socket/gRCP)</li>
<li>Promotion Engine will take in:
<ul>
<li>SkuInfo SubDocument</li>
<li>Profile Information</li>
<li>Cart Information
<ul>
<li>Coupons Ect</li>
</ul>
</li>
</ul>
</li>
<li>Essentially the same information that we are currently aggregating in PricingService/CheckoutService</li>
</ul>
<h3 id="boolean-expression-evaluator">Boolean Expression Evaluator</h3>
<ul>
<li>On several levels the expression evaluator needs to be able to tell if a given item (or cart) matches a promotion</li>
<li>This can be done using only the information provided by the input, and the information provided by the PMDL.</li>
<li>Since we have already determined the expression, and have separated each element into the various pieces as a type of Abstract Syntax tree - we only need a few specialized components to complete the transaction.</li>
</ul>
<ol>
<li>Abstract Syntax Trees for Qualifier/Quantifier/Offer</li>
<li>A method to perform an In-order traversal of the tree, verifying each node until we have determined if the given cart or items match the Qualifier/Quantifier/Offer</li>
<li>A Method to interpret each given condition property, e.g: <code>item.priceInfo.currencyCode == CAD</code>, <code>order.originOfOrder in [ios, andriod]</code></li>
</ol>
<p>In order to apply these, we can pull from the <a href="https://en.wikipedia.org/wiki/Interpreter_pattern">Interpreter Pattern</a> to build out our PMDL Interpreter.</p>
<h3 id="overall-architecture">Overall Architecture</h3>
<ul>
<li>When a promotion is first passed into the Promotion Engine, several processes will need to kick off.</li>
<li>The first portion of each order to be processed is the <code>Qualifier</code> which will determine if the order as a whole (shipping method, minimum spent, ect) qualifies the order for this promotion</li>
<li>The second portion to be processed is the quantifier - which will be processed on an item by item level - and determines if the promotion is valid based on the items available.</li>
<li>The third portion is the application of the offer - which can be done either at a
<ul>
<li>Shipping</li>
<li>Order</li>
<li>Item
<ul>
<li>This is the only level that requires the offer be processed by the boolean Expression Engine, and will apply at an item/item level</li>
</ul>
</li>
</ul>
</li>
<li>After this logic has been applied, the information available should be passed back on an item/order level to notify the FE of any discounts and their amount</li>
</ul>
<ol>
<li>Pre-process order and remove items which can not have a discount applied</li>
<li>Gather possible applicable promotions from mongo (Date/Avail/Site)
<ul>
<li>Check for Coupons to determine if any coupons are applicable</li>
<li>Filter possible promotions by Channel/SiteGroup/ect.</li>
</ul>
</li>
<li>Send each promotion and cart information to the Boolean Expression Evaluator to determine if the promotion is applicable</li>
<li>Determine from applicable promotions, which promotions will be applied to the cart
<ol>
<li>For each Applicable promotion on an item level, re-use logic evaluation engine to apply offer</li>
</ol>
</li>
<li>Re-Apply promotions which have been determined applicable, if they are multiple use</li>
<li>Update promotion usage statistics</li>
<li>Return New Order document to pricing engine</li>
<li>Check against email promotion coupon codes to filter</li>
</ol>
<p><img src="https://github.com/Alpha59/notes/blob/master/Untitled%20Diagram.jpg?raw=true" alt="Arch"></p>
<h3 id="future-improvements">Future Improvements</h3>
<p>A good future improvement would be to move the Evaluation into separate thread pools and utilize futures to ensure the the promotions are being evaluated as quickly as possible.</p>
<p>Another possible future improvement would be evaluating what single item (or item type) could be added to an order to trigger a promotion</p>
<h2 id="references">References</h2>
<p><a href="https://www.ibm.com/support/knowledgecenter/en/SSZLC2_7.0.0/com.ibm.commerce.customizetools.doc/concepts/cprengarch.htm">https://www.ibm.com/support/knowledgecenter/en/SSZLC2_7.0.0/com.ibm.commerce.customizetools.doc/concepts/cprengarch.htm</a><br>
<a href="http://web.cse.ohio-state.edu/software/2231/web-sw2/extras/slides/27.Recursive-Descent-Parsing.pdf">http://web.cse.ohio-state.edu/software/2231/web-sw2/extras/slides/27.Recursive-Descent-Parsing.pdf</a><br>
<a href="https://unnikked.ga/how-to-build-a-boolean-expression-evaluator-518e9e068a65">https://unnikked.ga/how-to-build-a-boolean-expression-evaluator-518e9e068a65</a><br>
<a href="https://unnikked.ga/the-shunting-yard-algorithm-36191ea795d9">https://unnikked.ga/the-shunting-yard-algorithm-36191ea795d9</a><br>
<a href="https://en.wikipedia.org/wiki/Interpreter_pattern">https://en.wikipedia.org/wiki/Interpreter_pattern</a><br>
<a href="https://github.com/silvershop/silvershop-core/issues/41">https://github.com/silvershop/silvershop-core/issues/41</a><br>
<a href="https://wiki.scn.sap.com/wiki/display/PLM/Deep+Dive+Promotion+Engine">https://wiki.scn.sap.com/wiki/display/PLM/Deep+Dive+Promotion+Engine</a></p>
<h2 id="appendix">Appendix</h2>
<p>An Exmple of the cart message (with discounts applied) can be found below:</p>
<pre><code>{
   "orderDuty":0.0,
   "convenienceCurrencyCode":"USD",
   "additionalShippingTotal":NumberInt(0),
   "total":172.8,
   "createdAt":   ISODate("2018-04-30T19:33:55.847+0000   "), 
    "   lastModified":   ISODate("2018-04-30T19:33:55.847+0000   "), 
    "   currencyCode":"USD",
   "shippingCost":0.0,
   "markers":{
      "iovationBlackBox":"XkBlHA1FfwWpaHcKDFrapYBo8JGxC5Z7J1hNC57nL7Q=",
      "languageCode":"en-US",
      "customerIpAddress":"10.27.10.22",
      "emailAddress":"jdirt1525116803559@mailinator.com",
      "reservationId":"0fb4d231-a375-472a-984c-645f7a821e79",
      "callCenterRepId":null
   },
   "discountSummary":{
      "totalDiscount":-7.95,
      "totalShippingDiscount":-7.95,
      "totalOrderDiscount":0.0,
      "totalItemDiscount":0.0
   },
   "sterlingOrderNumber":"F140002803",
   "channel":"web",
   "status":"ORDER_STATUS_PENDING",
   "shipBuckets":[
      {
         "selectedShippingOption":{
            "infoMessageCode":"US_Standard",
            "message":"US Standard FP",
            "shippingPriceInfo":{
               "discounted":true,
               "rawShipping":7.95,
               "currencyCode":"USD",
               "adjustments":[
                  {
                     "componentPrices":null,
                     "description":"Shipping Discount",
                     "atgCouponCode":null,
                     "amount":-7.95,
                     "quantityAdjusted":NumberInt(1),
                     "promotion":{
                        "promotionId":"promo240003",
                        "description":".COM, US + CAN + AUS - Free Standard Shipping; $100 Threshold",
                        "autoSelect":false,
                        "quantityAdjusted":null,
                        "unitDiscount":-7.95
                     }
                  }
               ],
               "amount":0.0,
               "amountIsFinal":false
            },
            "id":"400002",
            "name":"FPCOM_US_STANDARD"
         },
         "addressOwnerProfileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
         "hasFlatRate":false,
         "addressId":"5ae76f87478f4c00a7acfcb4",
         "itemIds":[
            "5ae76f68834026d517300dda"
         ],
         "type":"HARDGOOD",
         "addressOwnerDataCenterId":"US-PA",
         "marketPlace":false,
         "shippingGroupId":"sg20210053",
         "shippingOptionsRetrievalStatus":"SUCCESS",
         "shipAvailabilityGroups":[
            {
               "unavailableShippingOptions":[

               ],
               "items":[
                  "5ae76f68834026d517300dda"
               ],
               "shippingOptions":[
                  {
                     "name":"FPCOM_US_STANDARD",
                     "default":false,
                     "infoMessageCode":"US_Standard",
                     "shippingPriceInfo":{
                        "discounted":true,
                        "rawShipping":7.95,
                        "currencyCode":"USD",
                        "adjustments":[
                           {
                              "componentPrices":null,
                              "description":"Shipping Discount",
                              "atgCouponCode":null,
                              "amount":-7.95,
                              "quantityAdjusted":NumberInt(1),
                              "promotion":{
                                 "unitDiscount":-7.95,
                                 "quantityAdjusted":null,
                                 "autoSelect":false,
                                 "description":".COM, US + CAN + AUS - Free Standard Shipping; $100 Threshold",
                                 "promotionId":"promo240003"
                              }
                           }
                        ],
                        "amount":0.0,
                        "amountIsFinal":false
                     },
                     "serviceLevel":"80",
                     "message":"US Standard FP",
                     "shippingMethod":"400002",
                     "id":"400002",
                     "displayOrder":NumberInt(1)
                  },
                  {
                     "name":"FPCOM_US_EXPRESS",
                     "default":false,
                     "infoMessageCode":"US_Express",
                     "shippingPriceInfo":{
                        "discounted":false,
                        "rawShipping":10.0,
                        "currencyCode":"USD",
                        "adjustments":[

                        ],
                        "amount":10.0,
                        "amountIsFinal":false
                     },
                     "serviceLevel":"01",
                     "message":"US_Express",
                     "shippingMethod":"400003",
                     "id":"400003",
                     "displayOrder":NumberInt(2)
                  },
                  {
                     "name":"FPCOM_US_OVERNIGHT",
                     "default":false,
                     "infoMessageCode":"US_Overnight",
                     "shippingPriceInfo":{
                        "discounted":false,
                        "rawShipping":20.0,
                        "currencyCode":"USD",
                        "adjustments":[

                        ],
                        "amount":20.0,
                        "amountIsFinal":false
                     },
                     "serviceLevel":"52",
                     "message":"US Overnight FP",
                     "shippingMethod":"400004",
                     "id":"400004",
                     "displayOrder":NumberInt(3)
                  }
               ],
               "id":"5ae76f68834026d517300dda"
            }
         ],
         "taxAreaId":"391013000",
         "emailAddress":null,
         "shippingToPoBox":false,
         "address":{
            "city":"Philadelphia",
            "name":{
               "last":"John",
               "first":"Roszkowski"
            },
            "firstName":"Roszkowski",
            "toMask":false,
            "address1":"1132 E Hewson St",
            "region":"PA",
            "meta":{
               "profileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
               "archived":false,
               "nickName":null,
               "id":"5ae76f87478f4c00a7acfcb4",
               "visible":true
            },
            "phoneNumber":"2153332323",
            "lastName":"John",
            "postalCode":"19112",
            "country":"US",
            "id":"5ae76f87478f4c00a7acfcb4"
         },
         "message":null,
         "companion":null,
         "isMarketPlace":false,
         "id":"5ae76f9a834026d517300ddc",
         "method":"400002"
      }
   ],
   "orderAdjustments":[

   ],
   "rawSubtotal":160.0,
   "discountableSubtotal":160.0,
   "orderTax":12.8,
   "dataCenterId":"US-PA",
   "siteId":"fp-us",
   "user":{
      "guest":true,
      "loyaltyId":null,
      "loyaltySignupDate":null,
      "loyaltyChannelId":null,
      "profileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
      "email":null
   },
   "language":"en-US",
   "cartId":"0fb4d231-a375-472a-984c-645f7a821e79",
   "items":[
      {
         "fulfillmentSubtype":null,
         "commerceItemId":"ci759420070",
         "priceInfo":{
            "linePrice":160.0,
            "freightAmount":0.0,
            "dutyHtsCode":null,
            "lineListPrice":160.0,
            "adjustment":{
               "promotions":[

               ],
               "componentPrices":null,
               "unitDiscount":0.0,
               "atgCouponCode":null,
               "unitListPrice":80.0,
               "promotion":null
            },
            "flatRateShippingAmount":0.0,
            "unitNetPrice":80.0,
            "lineItemTaxAmount":12.8,
            "freightTaxAmount":0.0,
            "vatDetail":null,
            "lineDiscount":0.0,
            "unitListPrice":80.0,
            "lineOrderDiscount":0.0,
            "totalTaxAmount":12.8,
            "dutyVatAmount":0.0,
            "dutyDutyAmount":0.0,
            "lineOrderDiscountedPrice":160.0,
            "shippingChargeAmount":0.0
         },
         "reservationExpiration":NumberInt(1525117307),
         "reservations":[

         ],
         "deliveryDaysRangeEnd":null,
         "shipSeparately":false,
         "analytics":{
            "navigationId":"search"
         },
         "promisedDeliveryDate":null,
         "cutoffTime":null,
         "apologyEmailCode":null,
         "id":"5ae76f68834026d517300dda",
         "giftCard":null,
         "cartType":"TRANSACTIONAL",
         "shipGroupId":"5ae76f68834026d517300dda",
         "upgradeServiceLevel":null,
         "quantityReserved":NumberInt(2),
         "isFlatRate":false,
         "configuration":null,
         "categoryId":null,
         "productId":"FP-44795441-000",
         "skuId":"44795441",
         "firstAdded":1525116776.650715,
         "deliveryDaysRangeStart":null,
         "isGiftCard":false,
         "lastAdded":1525116776.650715,
         "dataCenterId":"US-PA",
         "registry":null,
         "giftWrapped":false,
         "eGiftCard":null,
         "itemStatus":"ALL_PROMISED",
         "madeToOrder":false,
         "wishlist":null,
         "marketPlace":false,
         "displayName":"Luminescent Crystal Case Rose/tyrien 7.",
         "cartId":"0fb4d231-a375-472a-984c-645f7a821e79",
         "lastModified":NumberInt(1525116827),
         "bucket":{
            "addressOwnerProfileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
            "addressId":"5ae76f87478f4c00a7acfcb4",
            "type":"HARDGOOD",
            "addressOwnerDataCenterId":"US-PA",
            "shippingMethod":"400002",
            "destinationCountry":"US"
         },
         "fulfillmentType":"DIRECT",
         "isEgiftCard":false,
         "inventoryStatus":"ALL_PROMISED",
         "poolId":"US_DIRECT",
         "profileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
         "giftWrap":null,
         "quantity":NumberInt(2)
      }
   ],
   "orderVatDetails":null,
   "payments":{
      "hasAuthorization":true,
      "creditCard":{
         "expirationMonth":"04",
         "creditCardToken":"1000976099000005",
         "cardType":"AMEX",
         "currencyCode":"USD",
         "lastFourDigits":"0005",
         "paymentStatus":{
            "authorizationCode":"087664",
            "cvvResponseCode":" ",
            "amount":172.8,
            "transactionTime":1525116835.285749,
            "transactionId":"2091c3ba70c1e9ee",
            "avsCode":"Y",
            "approved":true
         },
         "visible":true,
         "expirationYear":"2018",
         "verificationCode":"222",
         "address":{
            "city":"Philadelphia",
            "name":{
               "last":"John",
               "first":"Roszkowski"
            },
            "firstName":"Roszkowski",
            "lastName":"John",
            "region":"PA",
            "meta":{
               "profileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
               "archived":false,
               "nickName":null,
               "id":"5ae76f87478f4c00a7acfcb4",
               "visible":true
            },
            "phoneNumber":"2153332323",
            "country":"US",
            "postalCode":"19112",
            "address1":"1132 E Hewson St",
            "id":"5ae76f87478f4c00a7acfcb4"
         },
         "needsVerification":false,
         "approvedShippingAddresses":null,
         "id":"5ae76f94478f4c00ab19f182"
      }
   },
   "userCouponCode":null,
   "giftWrapTotal":0.0,
   "profileId":"4575367d-558d-4fa7-b8f3-dc4c248ab970",
   "amount":160.0,
   "realtimeTaxCalculated":true
}
</code></pre>
<p>An example of the cart response from PricingService can be seen below:</p>
<pre><code>{
   "count":1,
   "hasMergePending":false,
   "meta":{
      "totalCount":1,
      "totalQuantity":1
   },
   "user":{
      "profileId":"24de41df-1adf-4c92-9d73-fd4d672acdf0"
   },
   "cart":{
      "cartType":"TRANSACTIONAL",
      "priceInfo":{
         "duty":0.0,
         "realtimeTaxCalculated":false,
         "giftCardTotal":0,
         "shippingMethodSubtotal":0,
         "rawSubtotal":179.0,
         "currencyCode":"USD",
         "adjustments":[

         ],
         "tax":0.0,
         "orderVatDetails":null,
         "shipping":0,
         "discountableSubtotal":179.0,
         "discounts":{
            "totalDiscount":0.0,
            "totalShippingDiscount":0,
            "totalOrderDiscount":0.0,
            "totalItemDiscount":0.0
         },
         "amount":179.0,
         "remainingTotal":179.0,
         "additionalShippingTotal":0,
         "giftWrapTotal":0.0,
         "totalDiscountsWithGiftCards":0.0,
         "total":179.0,
         "totalRawtotalWithGiftwrap":179.0,
         "shippingCostCalculated":false,
         "totalTaxAndDuty":0.0
      },
      "cartId":"a796ba2f-92eb-4193-930e-5a7796d264af",
      "items":[
         {
            "fulfillmentSubtype":null,
            "commerceItemId":"ci3450510354",
            "priceInfo":{
               "linePrice":179.0,
               "freightTaxAmount":0.0,
               "lineOrderDiscountedPrice":179.0,
               "freightAmount":0.0,
               "dutyAmount":0.0,
               "flatRateShippingAmount":0.0,
               "unitNetPrice":179.0,
               "lineItemTaxAmount":0.0,
               "lineListPrice":179.0,
               "vatDetails":null,
               "lineDiscount":0.0,
               "unitListPrice":179.0,
               "lineOrderDiscount":0.0,
               "totalTaxAmount":0.0,
               "vatAmount":0.0,
               "adjustment":{
                  "promotions":[

                  ],
                  "componentPrices":null,
                  "unitDiscount":0.0,
                  "atgCouponCode":null,
                  "unitListPrice":179.0,
                  "promotion":null
               },
               "htsCode":null,
               "shippingChargeAmount":0.0
            },
            "reservations":null,
            "deliveryDaysRangeEnd":null,
            "shipSeparately":false,
            "analytics":{
               "navigationId":"new-clothes"
            },
            "apologyEmailCode":null,
            "id":"5b287c0eb7a1e000f23c1279",
            "cartType":"TRANSACTIONAL",
            "shipGroupId":null,
            "firstAdded":1529379854.656491,
            "isFlatRate":false,
            "displayName":"Deepv Red Flr Bttnfrnt Md Rd/rge Mot Xs",
            "lastAdded":1529379854.656491,
            "categoryId":null,
            "productId":"AN-4130099510029-000",
            "skuId":"46474474",
            "upgradeServiceLevel":false,
            "deliveryDaysRangeStart":null,
            "catalogInfo":{
               "product":{
                  "upholsterySwatch":false,
                  "parentCategoryId":"CLOTHES-DRESSES",
                  "additionalDescription":"* We have found this style runs small; we recommend sizing up for an ideal fit\n* Rayon\n* Deep v-neck\n* Tied sleeves\n* Midi silhouette\n* Button front\n* Hand wash\n* Imported",
                  "videoLink":"",
                  "navigationItemSlugs":[
                     "shop-all-dresses",
                     "new-dresses",
                     "dresses-maxi-midi",
                     "trend-boho-clothing",
                     "dresses-casual-everyday",
                     "shop-all-new",
                     "new-clothes",
                     "clothes-trend",
                     "shop-all-clothes-trend",
                     "dresses"
                  ],
                  "defaultSizeType":"REGULAR",
                  "categoryIds":[
                     "clothes-dresses-maxi",
                     "clothes-dresses",
                     "clothes-new-dresses",
                     "clothes-dresses-casual",
                     "clothes-new",
                     "clothes-features-shopByTrend",
                     "dresses-landing-page"
                  ],
                  "wallpaperCalculator":false,
                  "dimensions":"* Regular falls 45.25\" from shoulder",
                  "vintage":{
                     "howToWear":""
                  },
                  "displayForCountries":false,
                  "hasVideo":false,
                  "removeForLegalReasons":false,
                  "isFeaturedInACatalog":false,
                  "merchandiseClass":"4130",
                  "defaultImage":"4130099510029_069_b",
                  "productId":"AN-4130099510029-000",
                  "shippingAndReturns":false,
                  "availableForISPU":true,
                  "description":"Faithfull Yvette Dress",
                  "parentCategory":{
                     "displayName":"Dresses",
                     "id":"CLOTHES-DRESSES"
                  },
                  "newestColorDate":20180601,
                  "brand":"Faithfull",
                  "styleNumber":"4130099510029",
                  "ipProductName":"DEEPV RED FLR BTTNFRNT MD",
                  "isGiftCard":false,
                  "longDescription":"In a delightful floral print, this lightweight dress makes dressing for warmer days a breeze.\n\nDesigned with the modern traveler in mind, Faithfull pieces are feminine, flattering and versatile enough for wanderlust-fueled adventuring. Prints are inspired by vintage textiles and antique markets from across the world and each earth-toned garment is carefully produced using artisan techniques in Bali, Indonesia. \n\nFor fit advice, versatility tips or outfit inspiration, email [personalstylist@anthropologie.com](mailto:personalstylist@anthropologie.com) today.",
                  "isPetite":false,
                  "isActiveCatalog":false,
                  "includeInSearch":true,
                  "defaultColorCode":"069",
                  "supplierSku":"",
                  "requestSwatch":false,
                  "creationDate":1516128503.0,
                  "isMadeToOrder":false,
                  "wallpaperSwatch":false,
                  "webExclusive":true,
                  "preorderFlag":false,
                  "displayName":"Faithfull Yvette Dress",
                  "displaySoldOut":false,
                  "brandDescription":"",
                  "sizeGuide":"VENDOR_FAITHFULL",
                  "isVintage":false,
                  "configurationEnabled":false,
                  "isEgiftCard":false,
                  "navigationItems":[
                     "Clothing~The Trend Shops~'40s-Inspired Outfits",
                     "Clothing~New Arrivals",
                     "Clothing~New Arrivals~Shop All New Arrivals",
                     "Clothing~Dresses~Shop All Dresses",
                     "Clothing~New Arrivals~Dresses",
                     "Clothing~The Trend Shops~Shop All Trends",
                     "Clothing~The Trend Shops",
                     "Clothing~Dresses",
                     "Clothing~Dresses~Casual Dresses",
                     "Clothing~Dresses~Midi &amp; Maxi Dresses"
                  ],
                  "urbnExclusive":false,
                  "ipStyleNumber":"4130099510029",
                  "isAvailable":true,
                  "productSlug":"faithfull-yvette-dress",
                  "isMarketPlace":false,
                  "facets":{
                     "channels":[
                        "Android",
                        "POS",
                        "Web",
                        "iOS"
                     ],
                     "colors":[
                        {
                           "colorId":"069",
                           "colorways":[
                              {
                                 "hexColor":"#ffffff",
                                 "colorMappingSwatchURL":"",
                                 "name":"assorted",
                                 "colorMappingAlias":""
                              },
                              {
                                 "hexColor":"#d00010",
                                 "colorMappingSwatchURL":"",
                                 "name":"red",
                                 "colorMappingAlias":"red"
                              }
                           ]
                        }
                     ],
                     "vendorBrands":[
                        "Faithfull"
                     ]
                  }
               },
               "language":"en-US",
               "links":[
                  {
                     "locale":"en-US",
                     "productSlug":"faithfull-yvette-dress"
                  },
                  {
                     "locale":"fr-CA",
                     "productSlug":"faithfull-yvette-dress"
                  }
               ],
               "lastModified":1529356215,
               "currencyCode":"USD",
               "styleNumber":"4130099510029",
               "skuInfo":{
                  "allSkusMarkedDown":false,
                  "hasAvailableSku":true,
                  "listPriceLow":179.0,
                  "hasFlatRateSku":false,
                  "totalStockLevel":0,
                  "markdownState":"NONE",
                  "listPriceHigh":179.0,
                  "salePriceLow":179.0,
                  "hasMarkdown":false,
                  "investmentAmount":17184.0,
                  "salePriceHigh":179.0,
                  "secondarySlice":{
                     "displayLabel":"Size",
                     "sliceItems":[
                        {
                           "type":1,
                           "code":"REGULAR",
                           "displayName":"Regular",
                           "includedSizes":[
                              {
                                 "displayName":"XS",
                                 "id":"4000"
                              },
                              {
                                 "displayName":"S",
                                 "id":"5000"
                              },
                              {
                                 "displayName":"M",
                                 "id":"6000"
                              },
                              {
                                 "displayName":"L",
                                 "id":"7000"
                              },
                              {
                                 "displayName":"XL",
                                 "id":"8000"
                              }
                           ]
                        }
                     ],
                     "name":"size"
                  },
                  "displayListPrice":false,
                  "primarySlice":{
                     "displayLabel":"Color",
                     "sliceItems":[
                        {
                           "firstPublishedDate":"20180601",
                           "code":"069",
                           "displayName":"RED MOTIF",
                           "includedSkus":[
                              {
                                 "isDimeout":false,
                                 "discountable":true,
                                 "color":"RD/RGE MOT",
                                 "shipRestriction":null,
                                 "isDropShip":false,
                                 "markdownFlag":false,
                                 "smartSku":"4130099510029",
                                 "availabilityDate":1529712000,
                                 "size":"XS",
                                 "giftWrappable":true,
                                 "currencyCode":"USD",
                                 "hasShippingCharges":true,
                                 "afterpay":{
                                    "status":"AP_PROD_QUALIFY_MSG",
                                    "numOfPayments":4,
                                    "payment":44.75
                                 },
                                 "grade":"",
                                 "isFlatRate":false,
                                 "ipBrandCode":"01",
                                 "type":"REGULAR",
                                 "productId":"AN-4130099510029-000",
                                 "skuId":"46474474",
                                 "markdownState":"NONE",
                                 "overweightCharge":"0",
                                 "availableStatus":1003,
                                 "storeReturn":true,
                                 "siteId":"an-us",
                                 "sizeType":1,
                                 "sizeTypeCode":"REGULAR",
                                 "pool":"US_DIRECT",
                                 "shippingCharge":null,
                                 "displayName":"Deepv Red Flr Bttnfrnt Md Rd/rge Mot Xs",
                                 "parentProduct":null,
                                 "colorId":"069",
                                 "sizeId":"4000",
                                 "coordinateGroup":"",
                                 "stockLevel":0,
                                 "salePrice":179.0,
                                 "nonReturnable":false,
                                 "retailSku":"46474474",
                                 "backorder":44,
                                 "isAvailable":true,
                                 "listPrice":179.0
                              }
                           ],
                           "hexColor":"#fb4e26",
                           "productsRelatedToColor":[
                              {
                                 "viewCode":"b",
                                 "colorCode":"012",
                                 "styleNumber":"46709259",
                                 "productId":"AN-46709259-000"
                              },
                              {
                                 "viewCode":"b",
                                 "colorCode":"006",
                                 "styleNumber":"45330164",
                                 "productId":"AN-45330164-000"
                              },
                              {
                                 "viewCode":"b",
                                 "colorCode":"014",
                                 "styleNumber":"46543278",
                                 "productId":"AN-46543278-000"
                              }
                           ],
                           "stockLevel":0,
                           "hasVideo":false,
                           "swatchUrl":"",
                           "images":[
                              "b",
                              "b2",
                              "b3",
                              "b4"
                           ],
                           "colorways":[
                              {
                                 "hexColor":"#ffffff",
                                 "colorMappingSwatchURL":"",
                                 "name":"assorted",
                                 "colorMappingAlias":""
                              },
                              {
                                 "hexColor":"#d00010",
                                 "colorMappingSwatchURL":"",
                                 "name":"red",
                                 "colorMappingAlias":"red"
                              }
                           ],
                           "id":"4130099510029_069"
                        }
                     ],
                     "name":"color"
                  },
                  "afterpay":{
                     "status":"AP_PROD_QUALIFY_MSG",
                     "numOfPayments":4,
                     "payment":44.75
                  }
               },
               "reviews":{
                  "count":0,
                  "averageFit":0,
                  "siteGroup":"an-us",
                  "averageRating":0
               },
               "siteId":"an-us",
               "productSlug":"faithfull-yvette-dress",
               "id":"5b1166c24b55cb798a74f829",
               "pool":"US_DIRECT",
               "productId":"AN-4130099510029-000"
            },
            "isGiftCard":false,
            "madeToOrder":false,
            "dataCenterId":"US-PA",
            "multiLine":false,
            "registry":null,
            "giftWrapped":false,
            "eGiftCard":null,
            "itemStatus":null,
            "configuration":null,
            "wishlist":null,
            "marketPlace":false,
            "giftCard":null,
            "cartId":"a796ba2f-92eb-4193-930e-5a7796d264af",
            "lastModified":1529379854,
            "fulfillmentType":"DIRECT",
            "isEgiftCard":false,
            "poolId":"US_DIRECT",
            "profileId":"24de41df-1adf-4c92-9d73-fd4d672acdf0",
            "giftWrap":null,
            "quantity":1
         }
      ],
      "shippingDestinations":[

      ],
      "cartStatus":"ACTIVE",
      "afterpay":{
         "status":"AP_CART_QUALIFY_MSG",
         "numOfPayments":4,
         "payment":44.75
      }
   }
}
</code></pre>

