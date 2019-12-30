

## POST

**Description:** 
**Path:**   
**Method:**   
**Required Body Parameters:**   

 Parameter | Type   | Description 
 ---       |   ---  |          ---

**Optional Body Parameters:**   

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---


**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *body:* 
- *message:* Error message in case of an error 

**CURL Example**
```shell

```


## GET

**Description:**
**Path:**   
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return the VM with that ID. 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  
 - *message:* Error message in case of an error 

**Object format**
- *id* - The request’s id
- *status* - The request's current status. Options are pending/done/error
- *request* - An object that represents the request
- *error* - Error message if status is error

**CURL Example**
```shell

```