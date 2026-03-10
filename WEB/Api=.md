# What is an API?

An **API** — Application Programming Interface — is a set of rules that lets one software program talk to another. It defines *how* requests are made, *what* data you send, and *what* data you’ll get back.  
Think of it as a contract or a waiter who takes your order and brings back food from the kitchen.

---

## Real-life Example — Restaurant Analogy

1. You (client) sit at a table and look at the menu (API documentation).  
2. You tell the waiter (API request): “I want a cheeseburger and fries” (endpoint + parameters).  
3. The waiter delivers your order to the kitchen (server).  
4. Kitchen prepares food and gives it back to the waiter (server response).  
5. Waiter brings the food to you (API response).  

> If you want a secret menu item, the restaurant might require a password (API key / authentication).

---

## Technical Example — Weather API

Imagine an app that shows current weather. It calls a weather API endpoint and gets JSON data.

**Request (curl):**
```bash
curl "https://api.example.com/weather?city=London&units=metric&apikey=YOUR_API_KEY"

Rate limits = how many calls you can make per minute/hour.
```

Typical JSON response:

```
```{
  "city": "London",
  "temp": 12.3,
  "condition": "Light rain",
  "humidity": 81
}
````

Your app reads that JSON and displays: “12°C — Light rain”.

Quick points you should know

Endpoint = URL you call (e.g., /weather).

Method = HTTP verb (GET to read, POST to create, PUT to update, DELETE to remove).

Request body / params = the data you send.

Response = data returned (usually JSON).

Auth = API keys, tokens, OAuth to control access.

Rate limits = how many calls you can make per minute/hour.