# pyPricingAPI

The official chitta oracle (REST API)

pyPricing is the public available oracle to fetch latest pricing data for several crops, using data from the official eNAM site (eNAM.gov.in), and other on-chain sources. 

_As prices are manually entered, please be aware some typing errors issues may occur._ 


## overview

For now, the use of the API is limited to some APMCs (public agricultural markets) in Tamil Nadu. Please open a new issue if you would like access to any specific APMC, region or state in India.

FOR A FULL LIST OF AVAILABLE APMCS PLEASE SEE  https://github.com/ag-chitta/pyPricingAPI/blob/main/APMCs. (ONLY THOSE IN CAPITALS)

* **URL**

  https://k14y5popkj.execute-api.ap-south-1.amazonaws.com/stage/commodities

* **Method:**

  `GET`
  
*  **URL Params**

  **Required:**
 
   `place=string`
   
   The API requires a place locator indicator as a string. E.q 'Madurai', 'Chennai'. 
   
   **You can provide multiple places as a comma seperated list (e.g. 'Madurai,Chennai')**
 
   `radius=integer`.
   
   * The API requires a maximum distance indicator as a integer in KM (Kilometers). It will return all registered APMCs within a radius range of the place locator.
 
   `type=string`
   
   * The API can return both current and historical data. 
   
      * Set type='historical' to fetch timestamped series of minPrice, maxPrice, modalPrice and the commodities in store of a specific commodity ( + optional variety)
   
      * Set type='current' to only fetch the latest updated price fo the available commodity markets within the selected place/radius
  
   
   
   
   **State or National level data**
   
   It is possible to change the place locator to a state or national level, e.g. 'TAMIL NADU' or 'India'. The API will return weekly averaged pricing data of all    the available APMCs 
   
   In case 'current' is type='current', the highest and lowest price APMC per commodity is returned in the object string.
   
   
   
   
   **Optional:**
 
   `output=String`. (ONLY for type=current) _be aware of the capital in 'String'_
   
   * The API can return the data as a formatted string. This is usefull if you would like to present pricing data to your users in a chatbot type environment.

    Latest reported prices: 
 
    - PADDY : 1559 RS/Qui, CHETPET, 2022-02-11
    - GROUND NUT : 10124 RS/Qui, GINGEE, 2022-02-11
    - URAD (BLACK GRAM) : 6589 RS/Qui, GINGEE, 2022-02-11
    
  
   `language=string`. (ONLY for type=current)
   
   * The API can return the above output string in several languages:
   
      * _ENGLISH,TAMIL,_ (please open a issue to suggest additional languages)
   
  
   `commodities=string`
   
   * You can filter the output by commodity on the server-side using: begins_with:String. E.g. 'Paddy' or 'Maize'. It is not possible to filter the data on any variety
   



* **Success Response:**
  
  A succcess response will return the data as a JSON string.
  * **Code:** 200 <br />
    **Content:** `{ data : { value : DATA }`
    
    * Empty arrays generally indicate no listed APMCs have been found with the selected place/radius
    
    * "Latest reported prices: \n". indicate no listed APMCS have been found with the selected place/radius
    



* **Error Response:**

  * **Code:** 502 BAD GATEWAY <br />
    **Content:** `{ message : Internal server error" }`
    
  Please verify if you have the correct required inputs, and your inputs have the correct shape.



* **Sample Call:**
```
curl -X GET https://k14y5popkj.execute-api.ap-south-1.amazonaws.com/stage/commodities?radius=60&place=Arani&type=current
```

