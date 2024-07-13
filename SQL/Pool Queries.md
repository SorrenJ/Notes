
```js
const pool = new Pool({

  user: 'labber',

  password: 'labber',

  host: 'localhost',

  database: 'lightbnb'

});

  

// the following assumes that you named your connection variable `pool`

pool.query(`SELECT title

FROM properties

LIMIT 10;`

  

).then(response => {

  console.log(response)

})
```

- `getUserWithEmail`
    - Accepts an email address and will return a promise.
    - The promise should resolve with a user object with the given email address, or `null` if that user does not exist.


- Test that `getUserWithEmail` is working by trying to login as an existing user in the database. (Recall that every user's password is "password", _not_ the hashed version you see in the database.)
``` js


pool.query(`SELECT *

FROM users

WHERE email = $1;`

  

).then(response => {

  console.log(response)

})

```

- `getUserWithId`
    - Will do the same as `getUserWithEmail`, but using the user's `id` instead of `email`.


- Test that `getUserWithId` is working by refreshing the page while logged in. If you stay logged in, then this function is working.
``` js


pool.query(`SELECT *

FROM users

WHERE id = $1;`

  

).then(response => {

  console.log(response)

})

```


- `addUser`
    - Accepts a user object that will have a `name`, `email`, and `password` property
    - This function should insert the new user into the database.
    - It will return a promise that resolves with the new user object. This object should contain the user's `id` after it's been added to the database.
    - Add `RETURNING *;` to the end of an `INSERT` query to return the objects that were inserted. This is handy when you need the auto generated `id` of an object you've just added to the database.

- Test that `addUser` is working by signing up and then checking if the new user has been added to the database using `psql`.
``` js

  

const addUser = (user) => {

  const { name, password, email } = user;

  return pool

    .query('INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *',[name, email, password])

    .then((result) => {

      return result.rows[0];

    })

    .catch((error) => {

      console.error('Error executing addUser:', error);

      return null;

    });

};
```


We wrote a query for `getAllReservations` in [LightBnB Select](https://web.compass.lighthouselabs.ca/22cd02f7-b800-4006-b06f-c7515613abbd). The query we wrote in that activity is _almost_ the query we need to use here. However, that query returned only a few pieces of data (so that it could be easily seen in the terminal). If you were to use that query for this activity, it would not return all the data needed to render the list of reservations for the user on the front end. How can you change it to retrieve all of the necessary data?

Update the `getAllReservations` function to use the `lightbnb` database to return reservations associated with a specific user.

This function accepts a `guest_id`, limits the properties to 10 and returns a promise. The promise should resolve reservations for that user.

**Tip**: Copy the query you built in [LightBnB Select](https://web.compass.lighthouselabs.ca/22cd02f7-b800-4006-b06f-c7515613abbd) to use as a starting point, but alter it so that all necessary data is returned to render reservations correctly on the front end.



Original

``` js
const getAllReservations = function (guest_id, limit = 10) {

  return getAllProperties(null, 2);

};

```



```sql
SELECT reservations.id, properties.title, properties.cost_per_night, reservations.start_date, avg(rating) as average_rating
FROM reservations
JOIN properties ON reservations.property_id = properties.id
JOIN property_reviews ON properties.id = property_reviews.property_id
WHERE reservations.guest_id = 1
GROUP BY properties.id, reservations.id
ORDER BY reservations.start_date
LIMIT 10;
```




``` js
const getAllReservations => (userID) {
  const {r.id, p.title, p.cost_per_night, r.start_date } = userID;

  return pool

    .query('SELECT r.id, p.title, p.cost_per_night, r.start_date
    FROM reservations r
    JOIN properties p ON r.property_id = properties.id
    JOIN property_reviews pr ON p.id = pr.property_id
WHERE r.guest_id = $1
GROUP BY p.id, r.id
ORDER BY r.start_date
LIMIT 10;
    ')

    .then((result) => {

      return result.rows[0];

    })

    .catch((error) => {

      console.error('Error executing addUser:', error);

      return null;

    });

};

```



```js


// Define the function to get all reservations for a specific user
const getAllReservations = (userID) => {
  // SQL query to fetch reservation data
  const query = `
    SELECT r.id, p.title, p.cost_per_night, r.start_date, AVG(pr.rating) as average_rating
    FROM reservations r
    JOIN properties p ON r.property_id = p.id
    JOIN property_reviews pr ON p.id = pr.property_id
    WHERE r.guest_id = $1
    GROUP BY p.id, r.id
    ORDER BY r.start_date
    LIMIT 10;
  `;
  
  // Use an array to pass the userID securely as a parameter to the query
  const values = [userID];

  // Execute the query using the pool
  return pool.query(query, values)
    .then((result) => {
      // Return all rows from the query result
      return result.rows;
    })
    .catch((error) => {
      // Log any errors that occur during query execution
      console.error('Error executing getAllReservations:', error);
      return null;
    });
};

// Example usage of the function, passing a specific user ID (e.g., 1)
getAllReservations(1).then(reservations => console.log(reservations));


```




Property Listing


```sql
SELECT properties.id, title, cost_per_night, avg(property_reviews.rating) as average_rating

FROM properties

LEFT JOIN property_reviews ON properties.id = property_id

WHERE city LIKE '%ancouv%'

GROUP BY properties.id

HAVING avg(property_reviews.rating) >= 4

ORDER BY cost_per_night

LIMIT 10;
```



## WHERE

When `getAllProperties` is called, the options object can potentially contain the following properties:

```js
{
  city,
  owner_id,
  minimum_price_per_night,
  maximum_price_per_night,
  minimum_rating;
}
```

Note that the search options in the app do not include `owner_id`. However, it will be needed when you click "My Listings" in the app as `getAllProperties` will be used to implement both pieces of functionality.



If one of these properties has a value, we need to add it to the `WHERE` clause of our query. Let's see what this looks like with the `city` property.

### `city`

```js
const getAllProperties = function (options, limit = 10) {
  // 1
  const queryParams = [];
  // 2
  let queryString = `
  SELECT properties.*, avg(property_reviews.rating) as average_rating
  FROM properties
  JOIN property_reviews ON properties.id = property_id
  `;

  // 3
  if (options.city) {
    queryParams.push(`%${options.city}%`);
    queryString += `WHERE city LIKE $${queryParams.length} `;
  }

  // 4
  queryParams.push(limit);
  queryString += `
  GROUP BY properties.id
  ORDER BY cost_per_night
  LIMIT $${queryParams.length};
  `;

  // 5
  console.log(queryString, queryParams);

  // 6
  return pool.query(queryString, queryParams).then((res) => res.rows);
};
```


1. Setup an array to hold any parameters that may be available for the query.
2. Start the query with all information that comes before the `WHERE` clause.
3. Check if a city has been passed in as an option. Add the city to the params array and create a `WHERE` clause for the city.
    - We can use the length of the array to dynamically get the `$n` placeholder number. Since this is the first parameter, it will be `$1`.
    - The `%` syntax for the `LIKE` clause must be part of the parameter, not the query.
4. Add any query that comes after the `WHERE` clause.
5. Console log everything just to make sure we've done it right.
6. Run the query.

Allowing users to filter their results really complicates our query.


## Challenge


- if an `owner_id` is passed in, only return properties belonging to that owner.
- if a `minimum_price_per_night` and a `maximum_price_per_night`, only return properties within that price range. (**HINT**: The database stores amounts in cents, not dollars!)
- if a `minimum_rating` is passed in, only return properties with an average rating equal to or higher than that.

Remember that all of these may be passed in at the same time so they all need to work together. You will need to use `AND` for every filter after the first one. Also, none of these might be passed in, so the query still needs to work without a `WHERE` clause.


```js
const getAllProperties = function (options, limit = 10) {
  // 1
  const queryParams = [];
  // 2
  let queryString = `
  SELECT properties.*, avg(property_reviews.rating) as average_rating
  FROM properties
  JOIN property_reviews ON properties.id = property_id
  `;

  // 3
  if (options.city) {
    queryParams.push(`%${options.city}%`);
    queryString += `WHERE city LIKE $${queryParams.length} `;
  }

  // 4
  queryParams.push(limit);
  queryString += `
  GROUP BY properties.id
  ORDER BY cost_per_night
  LIMIT $${queryParams.length};
  `;

  // 5
  console.log(queryString, queryParams);

  // 6
  return pool.query(queryString, queryParams).then((res) => res.rows);
};
```


Step 1: Adding the owner_id Filter

Just after the city condition (if added), check if an owner_id was provided and needs to be filtered on:

```js
if (options.owner_id) {
  queryParams.push(`${options.owner_id}`);
  queryString += ` AND owner_id = $${queryParams.length} `;
}
```

Step 2: Handling the Price Range Filter

The price range filter involves both a minimum and maximum, so you'll check if either (or both) of these values are provided and add the appropriate conditions. Remember, the database stores the price in cents, so you'll need to adjust the input values (minimum_price_per_night and maximum_price_per_night) accordingly.

```js
if (options.minimum_price_per_night) {
  queryParams.push(options.minimum_price_per_night * 100); // Convert dollars to cents
  queryString += ` AND cost_per_night >= $${queryParams.length} `;
}
if (options.maximum_price_per_night) {
  queryParams.push(options.maximum_price_per_night * 100); // Convert dollars to cents
  queryString += ` AND cost_per_night <= $${queryParams.length} `;
}
```

Step 3: Implementing the minimum_rating Filter

Finally, you'll handle the minimum_rating by filtering on the average rating of each property:

```js
if (options.minimum_rating) {
  queryParams.push(options.minimum_rating);
  queryString += ` HAVING avg(property_reviews.rating) >= $${queryParams.length} `;
}
```


/lighthouse/LightBnB/LightBnB_WebApp


```js
const getAllProperties = function (options, limit = 10) {
  // 1
  const queryParams = [];
  // 2
  let queryString = `
  SELECT properties.*, avg(property_reviews.rating) as average_rating
  FROM properties
  JOIN property_reviews ON properties.id = property_reviews.property_id
  WHERE 1=1
  `;

  // 3
  if (options.city) {
    queryParams.push(`%${options.city}%`);
    queryString += ` AND city LIKE $${queryParams.length} `;
  }

  if (options.owner_id) {
    queryParams.push(`${options.owner_id}`);
    queryString += ` AND owner_id = $${queryParams.length} `;
  }

  if (options.minimum_price_per_night) {
    queryParams.push(options.minimum_price_per_night * 100); // Convert dollars to cents
    queryString += ` AND cost_per_night >= $${queryParams.length} `;
  }

  if (options.maximum_price_per_night) {
    queryParams.push(options.maximum_price_per_night * 100); // Convert dollars to cents
    queryString += ` AND cost_per_night <= $${queryParams.length} `;
  }

  // 4
  queryString += `
  GROUP BY properties.id
  `;

  if (options.minimum_rating) {
    queryParams.push(options.minimum_rating);
    queryString += ` HAVING avg(property_reviews.rating) >= $${queryParams.length} `;
  }

  queryString += `
  ORDER BY cost_per_night
  LIMIT $${queryParams.length + 1};
  `;

  queryParams.push(limit);

  // 5
  console.log(queryString, queryParams);

  // 6
  return pool.query(queryString, queryParams)
    .then((res) => res.rows)
    .catch((err) => {
      console.error(err);
      throw err;
    });
};

```




```js
const { Pool } = require('pg');
const pool = new Pool({
    user: 'yourDatabaseUser',
    host: 'yourDatabaseHost',
    database: 'yourDatabaseName',
    password: 'yourDatabasePassword',
    port: yourDatabasePort,
});

const addProperty = function (property) {
    const {
        owner_id,
        title,
        description,
        thumbnail_photo_url,
        cover_photo_url,
        cost_per_night,
        street,
        city,
        province,
        post_code,
        country,
        parking_spaces,
        number_of_bathrooms,
        number_of_bedrooms
    } = property;

    const queryString = `
        INSERT INTO properties (
            owner_id, 
            title, 
            description, 
            thumbnail_photo_url, 
            cover_photo_url, 
            cost_per_night, 
            street, 
            city, 
            province, 
            post_code, 
            country, 
            parking_spaces, 
            number_of_bathrooms, 
            number_of_bedrooms
        ) 
        VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11, $12, $13, $14)
        RETURNING *;
    `;

    const queryParams = [
        owner_id, 
        title, 
        description, 
        thumbnail_photo_url, 
        cover_photo_url, 
        cost_per_night, 
        street, 
        city, 
        province, 
        post_code, 
        country, 
        parking_spaces, 
        number_of_bathrooms, 
        number_of_bedrooms
    ];

    return pool.query(queryString, queryParams)
        .then(res => res.rows[0])
        .catch(err => console.error('query error', err.stack));
};
```


``` sql
CREATE TABLE guest_reviews (  
    id SERIAL PRIMARY KEY NOT NULL,
    rating int,
    message TEXT,
    guest_id INTEGER REFERENCES users(id) ON DELETE CASCADE,  
       owner_id INTEGER REFERENCES users(id) ON DELETE CASCADE,  
         reservation_id INTEGER REFERENCES reservations(id) ON DELETE CASCADE
);

```