const { test, expect } = require('@playwright/test');

let objectid;

 //Get list of all objects
test("Get list of all objects", async ({request}) => {
const response= await request.get('https://api.restful-api.dev/objects')
console.log(await response.json())
expect(response.status()).toBe(200)

const responseBody = await response.json();
console.log(responseBody);
expect(Array.isArray(responseBody)).toBeTruthy();
})

//Add an object using POST
test("Add an object", async ({request}) => {
const response = await request.post('https://api.restful-api.dev/objects',
                {
                "name": "Apple MacBook Pro 16",   
                data:{
                    "year": 2019,
                    "price": 1849.99,
                    "CPU model": "Intel Core i9",
                    "Hard disk size": "1 TB"
                 },
                Headers:{ "Accept":"application/json"}
                });
console.log(await response.json())
expect(response.status()).toBe(201)

var res= await response.json()
objectid=res.id

})

//Get a single object using the above added ID
test('Get a single object by ID', async ({request}) => {
    const response = await request.get('https://api.restful-api.dev/objects?id=3&id=5&id=10'+objectid,
    expect(response.status()).toBe(200));
    console.log(await response.json())
});


//Update the object using PUT
test("Update the object", async ({request}) => {
    const response = await request.put('https://api.restful-api.dev/objects/7'+objectid,
        {
            "name": "Apple MacBook Pro 16",
            "data": {
               "year": 2019,
               "price": 2049.99,
               "CPU model": "Intel Core i9",
               "Hard disk size": "1 TB",
               "color": "silver"
            },
        Headers:{ "Accept":"application/json"}
        });
console.log(await response.json())
expect(response.status()).toBe(200)
})

//Delete the object using DELETE
test("Delete the object", async ({request}) => {
   const response= await request.delete('https://api.restful-api.dev/objects/6'+objectid)
   expect(response.status()).toBe(204)

//Verify deletion by checking if the object still exists
   const verifyResponse = await request.get('https://api.restful-api.dev/objects/6'+objectid,);
   expect(verifyResponse.status()).toBe(404);
})
