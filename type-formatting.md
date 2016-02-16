##Type Formatting
###Boolean

A boolean is represented with **true** or **false**.

###Dates and Times

Use [**ISO 8601**](http://en.wikipedia.org/wiki/ISO_8601) format for passing in and out dates and times. Use [**UTC**](https://en.wikipedia.org/wiki/UTC) as timezone.

|Date|2015-07-02|
|---|---|
|Combined date and time in [**UTC**](https://en.wikipedia.org/wiki/UTC)|2015-07-02T14:47:47Z|
 

###Currency

Use [**3-character ISO-4217**](http://en.wikipedia.org/wiki/ISO_4217#Active_codes) codes for specifying currencies.

#####Samples
	 EUR - Euro  
	 CHF - Swiss franc  
	 USD - US Dollar  

###Country

Use country codes defined by the [**ISO 3166-1-alpha-2**](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code standard.

#####Samples
	 AD - Andorra  
	 DE - Germany  
	 FR - France  
	 US - United States of America  

###Amount

Use the format [0-9]+(\.[0-9]+)? to represent an amount like money.
Separate amount and currency in different fields.

#####Samples
	     12.23
	   3789.1
	  56782
	    -12.02
	   +231.98 

###UUID

A UUID or GUID is represented in the Format  '**dddddddd-dddd-dddd-dddd-dddddddddddd**' where each d represents [A-Fa-f0-9].

#####Samples
	 56e94f2b-25ac-4c58-9828-f63b66220999
	 2ABAD998-48F6-4847-9EB2-2A6B5328C51D

 
