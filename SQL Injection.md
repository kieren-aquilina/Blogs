<img src="https://miro.medium.com/v2/0*ErN7MyOU7wjQLSgM.jpg" alt="A description of the image" width="350">

<h2>Thu 19/06/2025</h2>

<p>Today on the way home from work I was listening to the 33rd episode of the podcast Darknet Diaries, by Jack Rysider<sup>1</sup> and this episode was about this man named Tom and how he discovered SQL injection vulnerabilities on multiple websites he had visited.</p>

<p>Tom, having experience in the IT field, had attempted to log into a website using a single quote. The website would go blank and display the error code "You Have an Error in Your SQL Syntax."<sup>2</sup> The episode continues down the path of discussing more about SQL injection and sanitising user input. This got me thinking and wanting to learn more. Currently, I'm in the process of completing Google's cybersecurity certificate on Coursera, and this topic of SQL injection has been covered but I wanted to do my own investigations to find out more about how SQL injections are done and what exactly sanitisation of user input is. So I did some digging.</p>

<p>According to codeacademy.com: “Through cleverly constructed text inputs that modify the backend SQL query, threat actors can force the application to output private data or respond in ways that provide intel.”<sup>2</sup> Even just reading the first few paragraphs of this webpage I’m amazed at the different methods, or rather, SQL injection techniques.</p>

<p>After reading the description of union-based injections and trying to understand the examples of code that they used to explain the definition of UNION SELECT, I was still left a little unsure that I understood what union-based injections are. My thoughts were “wait, this sounds a lot like an inner join query.” So, I decided to do a quick google search to find further explanations to help me understand what this means. What I did find was a really helpful diagram that gave a very simple yet easier way for me to understand what the difference was between UNION and JOIN queries, shown below.<sup>3</sup></p>

<img src="https://www.scaler.com/topics/images/sql-union-vs-join-thumbnail.webp" alt="A description of the image" width="450">


<p>Then after finding this diagram I began to read on from the ‘Scaler’ webpage article that this image originally came from. After reading through this information I have a far better understanding of the difference between UNION and JOIN. UNION is used when we want to display the results of two SELECT queries. This can be used to obtain usernames and passwords from two different tables. Consider an ecommerce website that stores passwords as simple strings that aren’t hashed, you could gather passwords and match them to usernames by using UNION to combine two SELECT statements. Note that with UNION the result does not contain any duplicated records. With JOIN you are extracting the data from more than one table and those records are combined into a new column, and that is the result. Put simply, UNION combines records of two SELECT statements’ results, and JOIN uses SELECT to combine as a result.</p>

<p>Reading the codeacademy article about error-based injections. This still doesn’t really explain in a way that I can understand, so I did another google search to find a better understanding of error-based injections. Throwback to earlier in this blog post when I mentioned that Tom used a single quote in the website's login form and got that error regarding syntax. I found a website that I recognised, owasp.org<sup>4</sup>, that continues to explain more about SQL injections with examples. Yes! I recognise this website as one of the resources from my time completing the Google certificate. So far this is one of the most creative ways to exploit databases online. From my understanding, if a threat actor is unable to get the information that they want using techniques like UNION they can use errors to display and extract the data that they want. The way in which this exploit is used is not in the log-in form but in the URL where adding a command after a standard query search will display the parameter added using two pipes. Here is an example of this in action:</p>

<pre><code>https://www.example.com/product.php?id=10||UTL_INADDR.GET_HOST_NAME( (SELECT user FROM DUAL) )--</code></pre>

<p>The threat actor is linking the value 10 with the result of the function added after the two pipes.

Next is boolean exploitation, which is useful in situations when a thread actor finds themself in a blind SQL injection position. This is where nothing is known about the outcome of a command. This means, no error or any other output from a command. As the name suggests you would need to observe the behaviour of the server using boolean operators. I used google again to try to dig more information into what boolean-based SQL injection is and I found an interesting article on invicti.com<sup>5</sup> writing about blind SQL injection. The examples used in the article were very helpful in understanding how boolean-based blind injection can be used to discover data in a database.</p>

<p>A threat actor could use a query to legitimately discover an existing product ID and then use that to exploit the database. For this example, a product ID of 42 was used.</p>

<code>SELECT * FROM products WHERE id = 42 and 1=1</code><br>
<code>SELECT * FROM products WHERE id = 42 and 1=0</code>

<p>The application is open to boolean-based SQL injections if these two commands behave differently. From here on, a threat actor can use boolean to go through all the letters of the alphabet to spell out the full name of the first table in the database. Yes, this takes a long time but the threat actor is still gathering information they can use to further exploit and get the data they require.</p>

<p>Just researching these few exploitation techniques has opened my eyes on how creative and persistent threat actors can be when they are desperate for information. This is what I love about cyber security, there is always something new to learn. In the best possible way, my head is spinning. Doing these sorts of small research projects and even this blog post allows me to further gain knowledge, sharpen my pencil on research techniques and ensure that I am taking in this information and not just reading and listening but actively making sure I understand this for when I eventually head out into the field and become a professional. I understand that there are far more techniques but will do so for now so as to not overload myself with information. I look forward to writing my next blog.</p>

<hr>


<sup>1</sup><a>https://darknetdiaries.com/episode/33/</a><br>
<sup>2</sup><a>https://www.codecademy.com/learn/seasp-defending-node-applications-from-sql-injection-xss-csrf-attacks/modules/seasp-preventing-sql-injection-attacks/cheatsheet</a><br>
<sup>3</sup><a>https://www.scaler.com/topics/sql-union-vs-join/</a><br>
<sup>4</sup><a>https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection</a><br>
<sup>5</sup><a>https://www.invicti.com/learn/blind-sql-injection/</a>
