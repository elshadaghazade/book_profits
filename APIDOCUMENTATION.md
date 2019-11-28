# API Documentation

For chrome extension we need some REST API endpoints. They are listed below.

## Price History Initial

This endpoint sends all price history data for amazon, new and used categories. At the same time on response should exists statistics information as shown in example below.

### Request:

<table>
    <tbody>
        <tr>
            <td>Endpoint:</td>
            <td colspan="9">/price_history/&lt;str:isbn&gt;/</td>
        </tr>
        <tr>
            <td>Method:</td>
            <td>GET</td>
        </tr>
        <tr>
            <td>Authentication:</td>
            <td>Cookie</td>
        </tr>
        <tr>
            <td>ISBN parameter:</td>
            <td>ISBN/ASIN of current book</td>
        </tr>
    </tbody>
</table>

### Success Response:

```js

{
    "status": 200,
    "error_message": null,
    "data": {
        "history": [
            {
                "when": 1574941564, // timestamp
                "value": {
                    "amazon": 30.833,
                    "new": 28.712,
                    "used": 31.567,
                    "sales_rank": 10456
                }
            },
            {
                "when": 1574941564, // timestamp
                "value": {
                    "amazon": 30.833,
                    "new": 28.712,
                    "used": 31.567,
                    "sales_rank": 10456
                }
            },
            ...
            {
                "when": 1574941564, // timestamp
                "value": {
                    "amazon": 30.833,
                    "new": 28.712,
                    "used": 31.567,
                    "sales_rank": 10456
                }
            }
        ],
        "statistics": {
            "amazon": {
                "lowest": 111.184,
                "current": 110.856,
                "highest": 123.454,
                "average": 95.566,
                "drops": 4
            },
            "new": {
                "lowest": 111.184,
                "current": 110.856,
                "highest": 123.454,
                "average": 95.566,
                "drops": 4
            },
            "used": {
                "lowest": 111.184,
                "current": 110.856,
                "highest": 123.454,
                "average": 95.566,
                "drops": 4
            },
            "sales_rank": {
                "lowest": 10000,
                "current": 20000,
                "highest": 30000,
                "average": 40000,
                "drops": 50000
            }
        }
    }
}
```

### Error Response:

```js

{
    "status": 500, // here should be appropriate HTTP status
    "error_message": "Something went wrong",
    "data": null
}
```

---

## Price History Additional

This endpoint sends all price history data for particular category (used like new, sales rank and etc.)

### Request:

<table>
    <tbody>
        <tr>
            <td>Endpoint:</td>
            <td colspan="9">/price_history/&lt;str:isbn&gt;/&lt;int:category number&gt;/</td>
        </tr>
        <tr>
            <td>Method:</td>
            <td>GET</td>
        </tr>
        <tr>
            <td>Authentication:</td>
            <td>Cookie</td>
        </tr>
        <tr>
            <td>ISBN parameter:</td>
            <td>ISBN/ASIN of current book</td>
        </tr>
        <tr>
            <td>Category number parameter:</td>
            <td>
                Category number parameter is integer. Meaning of each number is below:
                <ul>
                    <li>1 - used like new</li>
                    <li>2 - used very good</li>
                    <li>3 - used good</li>
                    <li>4 - used acceptable</li>
                    <li>5 - sales rank</li>
                    <li>6 - historical data (used & new count)</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

### Success Response:

```js

{
    "status": 200,
    "error_message": null,
    "data": {
        "history": [
            {
                "when": 1574941564, // timestamp
                "value": {
                    "category name with trailing underscores (for ex.: used_like_new or sales_rank)": 30.833
                }
            },
            {
                "when": 1574941564, // timestamp
                "value": {
                    "category name with trailing underscores (for ex.: used_like_new or sales_rank)": 30.833,
                }
            },
            ...
            {
                "when": 1574941564, // timestamp
                "value": {
                    "category name with trailing underscores (for ex.: used_like_new or sales_rank)": 30.833
                }
            }
        ]
    }
}
```

### Error Response:

```js

{
    "status": 500, // here should be appropriate HTTP status
    "error_message": "Something went wrong",
    "data": null
}
```

---

## Global and Individual Tracking Settings

This endpoint will be used for both sections: Tracking Individual Product & Global Tracking Settings. The endpoint accepts two type of request method: GET and PUT.

When endpoint receives GET method it should send back user's pre-saved individual and global tracking information at the same time on one single response.

When endpoint receives PUT method it means that backend should save/update sent data.

### Request with GET method:

<table>
    <tbody>
        <tr>
            <td>Endpoint:</td>
            <td colspan="9">/track_product/&lt;str:isbn&gt;/</td>
        </tr>
        <tr>
            <td>Method:</td>
            <td>GET</td>
        </tr>
        <tr>
            <td>Authentication:</td>
            <td>Cookie</td>
        </tr>
        <tr>
            <td>ISBN parameter:</td>
            <td>ISBN/ASIN of current book</td>
        </tr>
    </tbody>
</table>

### Success Response:

```js

{
    "status": 200,
    "error_message": null,
    "data": {
        "individual": {
            "isbn": "1617294543",
            "price": 67.65,
            "seller_rating": 56898,
            "seller_rating_percent": 100,
            "conditions": [1,2,3,4,5],
            "description": ["keyword", "keyword", ... "keyword"],
            "seller_name": ["keyword", "keyword", ... "keyword"],
            "ships_from_us_only": true
        },
        "global": {
            "seller_rating": 56898,
            "seller_rating_percent": 100,
            "conditions": [1,2,3,4,5],
            "description": ["keyword", "keyword", ... "keyword"],
            "seller_name": ["keyword", "keyword", ... "keyword"],
            "ships_from_us_only": true,
            "notification_email": "user@email.com"
        }
    }
}
```

***hint:*** *Meaning of conditions: 1 - new, 2 - used like new, 3 - used very good, 4 - used good, 5 - used acceptable*

### Request with PUT method:

<table>
    <tbody>
        <tr>
            <td>Endpoint:</td>
            <td colspan="9">/track_product/</td>
        </tr>
        <tr>
            <td>Method:</td>
            <td colspan="9">PUT</td>
        </tr>
        <tr>
            <td>Authentication:</td>
            <td colspan="9">Cookie</td>
        </tr>
        <tr>
            <td rowspan="12"><strong>PUT Parameters:</strong></td>
        </tr>
        <tr>
            <td><strong>Parameter name</strong></td>
            <td><strong>Value Type</strong></td>
            <td><strong>Is Array</strong></td>
            <td><strong>Description</strong></td>
        </tr>
        <tr>
            <td>section <em><strong>(required)</strong></em></td>
            <td>Integer</td>
            <td>False</td>
            <td>If value is 1 it means that data is sent for the tracking individual product section, if value is 2 then data is for global settings section.</td>
        </tr>
        <tr>
            <td>isbn <em><strong>(optional, but can be sent only if section value is 1)</strong></em></td>
            <td>String</td>
            <td>False</td>
            <td>ISBN/ASIN number of book</td>
        </tr>
        <tr>
            <td>price <em><strong>(optional, but can be sent only if section value is 1)</strong></em></td>
            <td>Float</td>
            <td>False</td>
            <td>Price less than</td>
        </tr>
        <tr>
            <td>seller_rating <em><strong>(optional)</strong></em></td>
            <td>Integer</td>
            <td>False</td>
            <td>Seller rating - min. #Feedback</td>
        </tr>
        <tr>
            <td>seller_rating_percent <em><strong>(optional)</strong></em></td>
            <td>Integer</td>
            <td>False</td>
            <td>Seller rating - Min. %</td>
        </tr>
        <tr>
            <td>condition <em><strong>(optional)</strong></em></td>
            <td>Integer</td>
            <td>True</td>
            <td>Condition value is any integer variated between 1 and 5. If used picked more that one condition then they will be sent as an array like: condition=1&condition=2&condition=5
            </td>
        </tr>
        <tr>
            <td>description <em><strong>(optional)</strong></em></td>
            <td>String</td>
            <td>True</td>
            <td>Description (does not contain).</td>
        </tr>
        <tr>
            <td>seller_name <em><strong>(optional)</strong></em></td>
            <td>String</td>
            <td>True</td>
            <td>Seller name (Is NOT).</td>
        </tr>
        <tr>
            <td>ships_from_us_only <em><strong>(optional)</strong></em></td>
            <td>Tinyint (Boolean)</td>
            <td>False</td>
            <td>Shipping Location - Ships from US only (if known). Values will be 1 or 0</td>
        </tr>
        <tr>
            <td>notification_email <em><strong>(optional)</strong></em></td>
            <td>String</td>
            <td>False</td>
            <td>User's email to get offers notifications</td>
        </tr>
    </tbody>
</table>

### Success Response:

```js

{
    "status": 200,
    "error_message": null,
    "data": null
}
```

### Error Response:

```js

{
    "status": 500, // here should be appropriate HTTP status
    "error_message": "Something went wrong",
    "data": null
}
```
