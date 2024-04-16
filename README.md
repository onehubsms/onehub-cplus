# Onehub C++ Library
```c++
using System.Net.Http;
using System.Net.Http.Headers;

HttpClient client = new HttpClient();

HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Post, "https://api.onehub.co.ke/v1/sms/send");

request.Headers.Add("x-api-user", "API_USERNAME_HERE");
request.Headers.Add("x-api-key", "API_KEY_HERE");

request.Content = new StringContent("{\"phoneNumbers\": \"+2547XXXXXXXX,+2547XXXXXXXX\", \"message\": \"Hello Api!\",\"senderId\":\"Onehub\"}");
request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");

HttpResponseMessage response = await client.SendAsync(request);
response.EnsureSuccessStatusCode();
string responseBody = await response.Content.ReadAsStringAsync();
```
# Username and API Key
You can get your username and API Key [here](https://dashboard.onehub.co.ke/account/0/user/signup).
# Request Body Parameters
`phoneNumbers` - `[Type: String]` `[Required]` - Phone numbers of the recipients in international format with the plus (+) sign. Multiple numbers should be comma-separated.

`Message` - `[Type: String]` `[Required]` - The message you want to send. Each message is counted as 160 characters long. Messages longer than 160 characters will be billed accordingly.

`Sender ID` - `[Type: String]` `[Required]` - A sender ID serves as a branding for outgoing messages, typically representing your company name. Your users will receive messages with your specified sender ID. You can apply for your sender ID on our [website](https://onehub.co.ke/).
# Response Body Parameters
## Response in case of successful sending of a message:
```json
{
    "0":{
        "country":"kenya",
        "cost":"KES 2.00",
        "success":[
            "+2547XXXYXXXX",
            "+2547XXXXXXXX"
        ]
    },
    "status":200,
    "message":"Message sent"
}
```
## Response in case a message fails:
```json
{
   "0":{
      "country":"kenya",
      "cost":"KES 1.00",
      "success":[
         "+2547XXXYXXXX"
      ],
      "failed":[
         "+2547XXXXXXXX"
      ]
   },
   "status":200,
   "message":"Message sent"
}
```
## Response in case of wrong credentials:
```json
{
    "status":401,
    "message":"Unauthorized"
}
```
