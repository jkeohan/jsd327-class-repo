# MVP
-Create a working website for **Thank Heaven** Children's Boutique.  
-Following a UX Design teams mock-up, create a page with necessary routes using React and Webpack.   
-Render products, brands, featured products and featured brands, images and brand stories.   
-Display reviews and contact information with contact form.   
-Use the Instagram API, and google maps API for store location and directions.   
-Complete all components using bootstrap and CSS.  

# POST-MVP

-Use factual data from client to seed in database.   
-Website Analytics.  
-Preview Product/Brand before posting in Admin Page.  
-E-commerce   
-Implementing Instagram Feed API to display clients images.  
-Style Admin Page   

# Installation Instructions
-Npm Install  
-Migrate and Seed the database, db name = thank-heaven  
-Npm Run Build  
-Admin Login is UserName: Test and Password: Test  
-To access Admin, in URL, add /admin    SUCH AS: [Thank Heaven Admin](https://thank-heaven.herokuapp.com/admin)

# Project Board
[Trello](https://trello.com/b/6SjDMAXb/thank-heaven)

# Link to Heroku
[Thank Heaven (on Heroku)](https://thank-heaven.herokuapp.com/)    

# App Components
## Header
Links to Facebook and Instagram and routes to each page.
## Footer
Links to Facebook and Instagram. Contact and Store information.
## Homepage
Header and Footer displayed here. Also, logo, welcome message, boutique information, carousel for new arrivals, images of featured products and featured brands, links to products and brand page, images from Instagram feed.
## Products Page
Header and Footer displayed here. Images and text of featured product items, it's corresponding brand, and a brief description. Images from Instagram feed.
## Brands Page
Header and Footer displayed here. Images and text of brands, where the brand is made, story behind the brand, a quote behind why the owner chose the brand and link to other products from that brand.
## Reviews Page
Reviews from Facebook
## Contact Page
A fixed image of store location on google maps, a link to directions to **Thank Heaven** using google maps, contact information, store hours and a contact form.

# Functional Components
|Component|Priority|EST time|Time Invested|Actual Time|
|---------|:------:|-------:|:-----------:|:---------:|
|Database | H   |2hrs    |  2hrs    |   2hrs     |
|Header/Footer| H | 2hrs   |   2hrs   |   2hrs     |
|Homepage |  H  |   6hrs|  4hrs  |   4hrs     |
|Products Page|  H  |   5hrs|     7hrs| 7hrs     |
|Brands Page|   H   |   5hrs|  8hrs| 8hrs    |
|Reviews    |   H   |   5hrs|   3hr  |    3hrs    |
|Contact    |   H    |   5hrs|  6hrs |  6hrs  |
|Bootstrap| H   | 5hrs|   8hrs    | 8hrs     |
|Instagram| H       |   3hrs|  2hrs  |    2hrs   |
|Google Maps API|H| 6hrs   | 1hr  |  1hr   |
|Forms      |   H       | 10hrs|    6hrs   |   6hrs   |
|Admin Page|    H    |16hrs  |  20hrs |    20hrs   |
|CSS         |  H |12hrs |   20hrs   |     20hrs   |
|Webpacks+Issues  |   H    | 4hr   |  3hr  | 3hrs    |
|Axios API Call |  H   | 3hrs   | 3hrs |  
|Auth       | H  | 1hr  |  6hrs  |  6hrs   |
|Pagination | H  | 2hr  |  3.5hrs  | 3.5hrs |



# Helper Functions
|Function|Description|
|-------:|:---------:|
|Pagination  | [Reference](http://jasonwatmore.com/post/2017/03/14/react-pagination-example-with-logic-like-google)|
|Make an Image with a white background see-through | mix-blend-mode: multiply; |



# Images
[Wireframes](http://res.cloudinary.com/jkarlin929/image/upload/v1517495773/THWireframes_jshjld.jpg)

[Component Hierarchy](http://res.cloudinary.com/jkarlin929/image/upload/v1517495768/THTree_ctmn6k.jpg)

[Admin Page/CRUD Functionality](http://res.cloudinary.com/jkarlin929/image/upload/v1517495751/THAdmin_yd4nfg.jpg)

[Database](http://res.cloudinary.com/jkarlin929/image/upload/v1517495757/THDB_anehmc.jpg)

[Time Matrix](http://res.cloudinary.com/jkarlin929/image/upload/v1517495763/THTimeMatrix_v4is4p.jpg)  

# Issues and Resolutions
To submit contact form, using webpack and react, nodemailer is not compatable because it uses templates in express. Need to build on front-end only. Ended up using formspree.io for a form to send e-mail to client.

Webpacks comes along with many issues, including uploading local images to display. This particular problem took 2.5 hours to solve, and uses url-loader and file-loader, to deal with small and large files respectively. The solution found was located here and was very thorough and helpful:
[Webpack Solution](https://medium.com/a-beginners-guide-for-webpack-2/handling-images-e1a2a2c28f8d)

Another issue related to webpacks was being unable to refresh and load the correct page or directly access a url that wasn't the root level. The solutions floating around the internet seemingly did not help, and dealt mostly with solving it on the webpack side, which included using a npm package called webpack-dev-server and perhaps many other related middleware. After 3.5 hours, the solution, or workaround rather, was to change the express backend to directly serve the files needed to each individual link.

-HTTP request not returning content-range, have tried to find a solution to add custom header to send the json data with the header
-Create an additional “Headers” object which looks like this

```
res.json({
        headers: {
          'Content-Type': 'application/json;',
          'Content-Range': reviews.length,
        }
        data: {
          message: 'ok',
          data: reviews,
      }
```

and then i’ve tried to add it to the app.get as suggested in the node.js docs as well as SO
```
app.get('/reviews', (req, res) => {
  res.sendFile(path.join(__dirname + '/index.html'))
  res.set('Content-Range', '4');
});
```

Route react-router-dom component, if you use the render function, it will always take those props, regardless of how far down nested the component it's ` linking to actually is, and will also override any props that you are trying to pass through to it from else where. There was an issue of trying to use a Link to pass through proper props, but it was being overwritten by the Route in the App.js.


On working with the admin page, I took a crack at it. First step was to take a glance at the documentation, and determine how the actual package works. The documentation was pretty well maintained and coherent. So after messing around with using different methods described in the documentation, I changed back to one of the original methods, and played around with using placeholder json from the site:
[Placeholder Site](http://jsonplaceholder.typicode.com)

Json data was most definitely coming through on our end, and for some reason, it either couldn't pull the header information from our localhost that we posted from our database.

My first guess was that the json data that was being accessed by the admin-on-rest package needed to be in a specific format, and our json data was actually nested inside another object, which while it didn't solve our current problem, was another one that would have popped up if I didn't fix.

Since that was the case, I re-setup our controllers to pull data from the placeholder site, and then repush that information onto our localhost, and lo and behold I was learning grabbing our data from our localhost was messing something up. It looked like it was either not pulling the header information or mutating it. After several non-working solutions I came across this post that wasn't directly related to admin-on-rest:
[Link to Github](https://github.com/axios/axios/issues/1255#issuecomment-354090991)

Luckily it seemingly had a solution and was even tailored to express as a lot of the solutions seemed to be made for ruby, however this worked to create, or set the value of the header that could then be reached through from our localhost. I then created the needed X-Content-Length header, and data was being pulled. This was the last big hump in getting our admin page working.

Trying to validate password vs/ hash using bscrypt.js. was using compareSync and even though the console showed that the two were identical, it was returning false and failing. Using async with bscript’s “compare” fixed the issue. Reading the docs on understanding tokens which works differently than sessions

# References

[For styling e-mail link in Contact Page](https://www.webdesignerdepot.com/2014/05/8-simple-css3-transitions-that-will-wow-your-users/)
