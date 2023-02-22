---
description: Algonquin College CST8288 note
---

# Servlets

## 2- Tiered Applications

"client server"：tiers communicate via network protocols， e.g. using JDBC&#x20;

where client implements: presentation, business rules/logic and data access logic

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption><p>Week 6 PPT - P3</p></figcaption></figure>

## Multi-tiered Applications&#x20;

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption><p>Week 6 PPT - P4</p></figcaption></figure>

in N- tier model&#x20;

* presentation tier: implemented on client device  and using content (e. HTML) provided by "application" server&#x20;
* business tier: creates presentation content  and implements business rules/logic on an "application" server&#x20;
* data tier: implements data access logic on the "application" server  and  data stored on "database" server &#x20;
  * Notes: servers may be replicated for redundancy and load&#x20;

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption><p>Week 6 PPT - P6</p></figcaption></figure>

## Servlet - lifecycle

`init()` and `destroy()` from class HttpServlet&#x20;

* `init()` method called when servlet first created  bur not called again as long as the servlet is not destroyed.&#x20;
* `destroy()` method is invoked once:
  * all in `service()` method have exited&#x20;
  * or after timeout period&#x20;
  * releases container resources that were allocated.&#x20;

`service()` invoked each time server receives request for servlet.&#x20;

* server spawns a new thread and invokes `service()`.
* based on request type, call appropriate method&#x20;
* GET , POST requests go to: `doGet() doPost()` are the most common ones
* &#x20;provides default handlers so _**do not override service()**_ • override specific `doXXX()` method instead

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption><p>Week 6 PPT - P9</p></figcaption></figure>

## classes: HttpServletRequest\&HttpServletResponse

protected void doXXX(HttpServletRequest req, HttpServletResponse resp)&#x20;

* servlet obtains request from HTTPServletRequest and creates response using HTTPServletResponse&#x20;

these 2 classes encapsulated request response • doXXX() methods throws following: `ServletException, java.io.IOException`

## Servlet overview of coding

create servlet by: public class \_\_\_\_\_ extends HttpServlet&#x20;

then override specific doXXX methods&#x20;

* if using same code for doGet() and doPost()&#x20;
* then have one call the other or have both call a common method e.g. in NetBeans skeleton code both doGet() and doPost() call processRequest()

<pre class="language-java"><code class="lang-java">protected void processRequest(HttpServletRequest request, 
HttpServletResponse response)
    throws ServletException,IOException{
<strong>response.setContentType("text/html;charset=UTF 8");
</strong>
try(PrintWriter out = response.getWriter()) {
    out.println("&#x3C;!DOCTYPE html>");
    out.println("&#x3C;html>");
    out.println("&#x3C;head>");
    /* out.println() statements for head section */
    out.println("&#x3C;/head>");
    out.println("&#x3C;body>");
    /* more out.println() statements for body section */
    out.println("&#x3C;/body>");
    out.println("&#x3C;/html>");
    }    
}
</code></pre>

## Servlet - troubleshooting&#x20;

the browser caches results: browser feature: reload without cache&#x20;

examine HTML source: browser features: show source&#x20;

display details of response and/or request&#x20;

* both classes have methods that get this info&#x20;
* echo to Web page or to log file

logging `void log(String msg)` / `void log(String msg, Throwablet)`&#x20;

* will write to the application server's log: log location depends on implementation • NetBeans shows server's log in window at bottom
* &#x20;if date time needed use: `DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss"); log(dtf.format(LocalDateTime.now()) some_msg");`&#x20;
* or use logging framework like Apache Log4j

## Servlet class hierarchy from

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption><p>Week 6 PPT - P16</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption><p>Week 6 PPT - P17</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption><p>Week 6 PPT - P18</p></figcaption></figure>
