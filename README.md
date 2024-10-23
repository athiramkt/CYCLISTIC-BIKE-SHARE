<html>
  <head></head>
  <body>
  <h1 id="cyclistic-bike-share-case-study-with-r">CYCLISTIC BIKE SHARE
CASE STUDY WITH R</h1>
  <h2 id="introduction">INTRODUCTION</h2>
    <blockquote>
      <p>This capstone project serves as the culmination of my Google Data Analytics Professional Certificate program. To effectively analyze and visualize the data, I have chosen to use the R programming language in conjunction with the RStudio Integrated Development Environment (IDE). R's robust statistical analysis capabilities and RStudio's user-friendly interface make it an ideal tool for this endeavor.</p>
    </blockquote>
<p>
<h2 id="-table-of-contents"> Table Of Contents</h2>
  <ul>
    <li>
     <a href="#scenario">SCENARIO</a>  
    </li>
    <li>
      <a href="#ask">ASK</a>
    </li>
    <li>
      <a href="#prepare">PREPARE</a>
    </li>
    <li>
      <a href="#process">PROCESS</a>
    </li>
    <li>
      <a href="#analyze">ANALYZE</a>
    </li>
    <li>
      <a href="#act">ACT</a>
    </li>
  </ul>
  <h2 id="scenario">SCENARIO</h2>
    <blockquote>
      <p>
      Cyclistic, a Chicago-based bike-share company, serves two types of customers: casual riders and members. Casual riders are those who purchase single-ride or full-day passes, while members are those with annual subscriptions. Finance analysts have determined that annual members are significantly more profitable than casual riders. The Director of Marketing believes that increasing the number of annual members is crucial for the company’s long-term success and growth.<br> </br> </p>
<p> The marketing analytics team aims to explore differences in bike usage between casual riders and members to develop targeted strategies for converting casual riders into annual members.The primary stakeholders include the Director of Marketing and the Cyclistic Executive Team. As a newly joined Junior Data Analyst on the Cyclistic Marketing Analytics Team, your role is to identify trends and analyze customer bike usage to support the company’s goals.
</p>
</blockquote>
<h2 id="ask">ASK</h2>
<blockquote>
<ul>
<li>
  Question:
  <ol type="1">
  <li>
   What is the difference between Subscriber and Custumer using Cyclistic bikes?
  </li>
  <li>
    Why did Custumer buy an annual Cyclistic membership?
  </li>
  <li>
     How can Cyclistic use digital media to influence casual riders to become members?
  </li>
 </ol>
</li>  
</ul>
</blockquote>  
<h2 id="prepare">PREPARE</h2>
<p>
  Dataset: 
  <a href="https://divvy-tripdata.s3.amazonaws.com/index.html">https://divvy-tripdata.s3.amazonaws.com/index.html</a> 
</p>
<ul>
  <li> 
    For this analysis, I will use Q1 2019 to Q4 2019 data.
  </li>
</ul>
<div class="sourceCode" id="cb1"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb1-1"><a href="#cb1-1" aria-hidden="true" tabindex="-1"></a><span class="co">#install packages </span></span>
<span id="cb1-2"><a href="#cb1-2" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(tidyverse)</span>
<span id="cb1-3"><a href="#cb1-3" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(lubridate)</span>
<span id="cb1-4"><a href="#cb1-4" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(ggplot2)</span>
<span id="cb1-5"><a href="#cb1-5" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(dplyr)</span>
<span id="cb1-6"><a href="#cb1-6" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(knitr)</span></code></pre></div>
    
 <pre>
   <code> 
     ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
 ✔ dplyr     1.1.1     ✔ readr     2.1.4
 ✔ forcats   1.0.0     ✔ stringr   1.5.0
 ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
 ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
 ✔ purrr     1.0.1     
 ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
 ✖ dplyr::filter() masks stats::filter()
 ✖ dplyr::lag()    masks stats::lag()
 ℹ Use the ]8;;http://conflicted.r-lib.org/conflicted package]8;; to force all conflicts to become errors</code></pre>   

  <div class="sourceCode" id="cb3"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb3-1"><a href="#cb3-1" aria-hidden="true" tabindex="-1"></a><span class="co">#import datasets</span></span>
<span id="cb3-2"><a href="#cb3-2" aria-hidden="true" tabindex="-1"></a>q1_2019 <span class="ot">&lt;-</span> <span class="fu">read_csv</span>(<span class="st">"Divvy_Trips_2019_Q1.csv"</span>)</span></code></pre></div>
<pre>
<code> 
Rows: 365069 Columns: 12
 ── Column specification ────────────────────────────────────────────────────────
 Delimiter: ","
 chr  (4): from_station_name, to_station_name, usertype, gender
 dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
 num  (1): tripduration
 dttm (2): start_time, end_time
 ℹ Use `spec()` to retrieve the full column specification for this data.
 ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.</code></pre>

<div class="sourceCode" id="cb5"><pre class="sourceCode r">
     <code class="sourceCode r">
       <span id="cb5-1"><a href="#cb5-1" aria-hidden="true" tabindex="-1"></a>q2_2019 <span class="ot">&lt;-</span> <span class="fu">read_csv</span>(<span class="st">"Divvy_Trips_2019_Q2.csv"</span>)</span></code></pre></div>
       <pre><code> Rows: 1108163 Columns: 12
 ── Column specification ────────────────────────────────────────────────────────
 Delimiter: ","
 chr  (4): 03 - Rental Start Station Name, 02 - Rental End Station Name, User...
 dbl  (5): 01 - Rental Details Rental ID, 01 - Rental Details Bike ID, 03 - R...
 num  (1): 01 - Rental Details Duration In Seconds Uncapped
 dttm (2): 01 - Rental Details Local Start Time, 01 - Rental Details Local En...
 ℹ Use `spec()` to retrieve the full column specification for this data.
 ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.</code></pre>
 
<div class="sourceCode" id="cb7"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb7-1"><a href="#cb7-1" aria-hidden="true" tabindex="-1"></a>q3_2019 <span class="ot">&lt;-</span> <span class="fu">read_csv</span>(<span class="st">"Divvy_Trips_2019_Q3.csv"</span>)</span></code></pre></div>
 <pre>
 <code>
 Rows: 1640718 Columns: 12
── Column specification ────────────────────────────────────────────────────────
   Delimiter: ","
   chr  (4): from_station_name, to_station_name, usertype, gender
   dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
   num  (1): tripduration
   dttm (2): start_time, end_time
   ℹ Use `spec()` to retrieve the full column specification for this data.
   ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.</code></pre> 
  
<div class="sourceCode" id="cb9"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb9-1"><a href="#cb9-1" aria-hidden="true" tabindex="-1"></a>q4_2019 <span class="ot">&lt;-</span> <span class="fu">read_csv</span>(<span class="st">"Divvy_Trips_2019_Q4.csv"</span>)</span></code></pre></div>
<pre><code> Rows: 704054 Columns: 12
 ── Column specification ────────────────────────────────────────────────────────
 Delimiter: ","
 chr  (4): from_station_name, to_station_name, usertype, gender
 dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
 num  (1): tripduration
 dttm (2): start_time, end_time
 ℹ Use `spec()` to retrieve the full column specification for this data.
 ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.</code></pre>
<h2 id="process">PROCESS</h2>  
<p> We need to check the column names first before merging the four datasets. This is important because all column names must be the same.</p>

<div class="sourceCode" id="cb11"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb11-1"><a href="#cb11-1" aria-hidden="true" tabindex="-1"></a><span class="fu">colnames</span>(q1_2019)</span></code></pre></div>
<pre><code>  [1] "trip_id"           "start_time"        "end_time"         
  [4] "bikeid"            "tripduration"      "from_station_id"  
  [7] "from_station_name" "to_station_id"     "to_station_name"  
 [10] "usertype"          "gender"            "birthyear"</code></pre>
 <div class="sourceCode" id="cb13"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb13-1"><a href="#cb13-1" aria-hidden="true" tabindex="-1"></a><span class="fu">colnames</span>(q2_2019)</span></code></pre></div>
 <pre><code>  [1] "01 - Rental Details Rental ID"                   
  [2] "01 - Rental Details Local Start Time"            
  [3] "01 - Rental Details Local End Time"              
  [4] "01 - Rental Details Bike ID"                     
  [5] "01 - Rental Details Duration In Seconds Uncapped"
  [6] "03 - Rental Start Station ID"                    
  [7] "03 - Rental Start Station Name"                  
  [8] "02 - Rental End Station ID"                      
  [9] "02 - Rental End Station Name"                    
 [10] "User Type"                                       
 [11] "Member Gender"                                   
 [12] "05 - Member Details Member Birthday Year"</code></pre>
 <div class="sourceCode" id="cb15"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb15-1"><a href="#cb15-1" aria-hidden="true" tabindex="-1"></a><span class="fu">colnames</span>(q3_2019)</span></code></pre></div>
 <pre><code>  [1] "trip_id"           "start_time"        "end_time"         
  [4] "bikeid"            "tripduration"      "from_station_id"  
  [7] "from_station_name" "to_station_id"     "to_station_name"  
 [10] "usertype"          "gender"            "birthyear"
 </code></pre>
  </p>   
  </body>
  <pre><code>  [1] "trip_id"           "start_time"        "end_time"         
  [4] "bikeid"            "tripduration"      "from_station_id"  
  [7] "from_station_name" "to_station_id"     "to_station_name"  
 [10] "usertype"          "gender"            "birthyear"</code></pre>
 <div class="sourceCode" id="cb17"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb17-1"><a href="#cb17-1" aria-hidden="true" tabindex="-1"></a><span class="fu">colnames</span>(q4_2019)</span></code></pre></div>
 <pre><code>  [1] "trip_id"           "start_time"        "end_time"         
  [4] "bikeid"            "tripduration"      "from_station_id"  
  [7] "from_station_name" "to_station_id"     "to_station_name"  
 [10] "usertype"          "gender"            "birthyear"</code></pre>
 <p>Since the columns do not match those in other datasets, we will proceed by renaming them.</p>
 <div class="sourceCode" id="cb19"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb19-1"><a href="#cb19-1" aria-hidden="true" tabindex="-1"></a>q2_2019fixed <span class="ot">&lt;-</span> <span class="fu">rename</span>(q2_2019</span>
<span id="cb19-2"><a href="#cb19-2" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ride_id =</span> <span class="st">"01 - Rental Details Rental ID"</span></span>
<span id="cb19-3"><a href="#cb19-3" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">rideable_type =</span> <span class="st">"01 - Rental Details Bike ID"</span> </span>
<span id="cb19-4"><a href="#cb19-4" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">started_at =</span> <span class="st">"01 - Rental Details Local Start Time"</span>  </span>
<span id="cb19-5"><a href="#cb19-5" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ended_at =</span> <span class="st">"01 - Rental Details Local End Time"</span>  </span>
<span id="cb19-6"><a href="#cb19-6" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_name =</span> <span class="st">"03 - Rental Start Station Name"</span> </span>
<span id="cb19-7"><a href="#cb19-7" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_id =</span> <span class="st">"03 - Rental Start Station ID"</span></span>
<span id="cb19-8"><a href="#cb19-8" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_name =</span> <span class="st">"02 - Rental End Station Name"</span> </span>
<span id="cb19-9"><a href="#cb19-9" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_id =</span> <span class="st">"02 - Rental End Station ID"</span></span>
<span id="cb19-10"><a href="#cb19-10" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">tripduration =</span> <span class="st">"01 - Rental Details Duration In Seconds Uncapped"</span></span>
<span id="cb19-11"><a href="#cb19-11" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">birthyear =</span> <span class="st">"05 - Member Details Member Birthday Year"</span></span>
<span id="cb19-12"><a href="#cb19-12" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">gender =</span> <span class="st">"Member Gender"</span></span>
<span id="cb19-13"><a href="#cb19-13" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">member_casual =</span> <span class="st">"User Type"</span>)</span>
<span id="cb19-14"><a href="#cb19-14" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb19-15"><a href="#cb19-15" aria-hidden="true" tabindex="-1"></a>q4_2019fixed <span class="ot">&lt;-</span> <span class="fu">rename</span>(q4_2019</span>
<span id="cb19-16"><a href="#cb19-16" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ride_id =</span> trip_id</span>
<span id="cb19-17"><a href="#cb19-17" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">rideable_type =</span> bikeid </span>
<span id="cb19-18"><a href="#cb19-18" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">started_at =</span> start_time  </span>
<span id="cb19-19"><a href="#cb19-19" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ended_at =</span> end_time  </span>
<span id="cb19-20"><a href="#cb19-20" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_name =</span> from_station_name </span>
<span id="cb19-21"><a href="#cb19-21" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_id =</span> from_station_id </span>
<span id="cb19-22"><a href="#cb19-22" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_name =</span> to_station_name </span>
<span id="cb19-23"><a href="#cb19-23" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_id =</span> to_station_id </span>
<span id="cb19-24"><a href="#cb19-24" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">member_casual =</span> usertype)</span>
<span id="cb19-25"><a href="#cb19-25" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb19-26"><a href="#cb19-26" aria-hidden="true" tabindex="-1"></a>q3_2019fixed <span class="ot">&lt;-</span> <span class="fu">rename</span>(q3_2019</span>
<span id="cb19-27"><a href="#cb19-27" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ride_id =</span> trip_id</span>
<span id="cb19-28"><a href="#cb19-28" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">rideable_type =</span> bikeid </span>
<span id="cb19-29"><a href="#cb19-29" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">started_at =</span> start_time  </span>
<span id="cb19-30"><a href="#cb19-30" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ended_at =</span> end_time  </span>
<span id="cb19-31"><a href="#cb19-31" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_name =</span> from_station_name </span>
<span id="cb19-32"><a href="#cb19-32" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_id =</span> from_station_id </span>
<span id="cb19-33"><a href="#cb19-33" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_name =</span> to_station_name </span>
<span id="cb19-34"><a href="#cb19-34" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_id =</span> to_station_id </span>
<span id="cb19-35"><a href="#cb19-35" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">member_casual =</span> usertype)</span>
<span id="cb19-36"><a href="#cb19-36" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb19-37"><a href="#cb19-37" aria-hidden="true" tabindex="-1"></a>q1_2019fixed <span class="ot">&lt;-</span> <span class="fu">rename</span>(q1_2019</span>
<span id="cb19-38"><a href="#cb19-38" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ride_id =</span> trip_id</span>
<span id="cb19-39"><a href="#cb19-39" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">rideable_type =</span> bikeid </span>
<span id="cb19-40"><a href="#cb19-40" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">started_at =</span> start_time  </span>
<span id="cb19-41"><a href="#cb19-41" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">ended_at =</span> end_time  </span>
<span id="cb19-42"><a href="#cb19-42" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_name =</span> from_station_name </span>
<span id="cb19-43"><a href="#cb19-43" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">start_station_id =</span> from_station_id </span>
<span id="cb19-44"><a href="#cb19-44" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_name =</span> to_station_name </span>
<span id="cb19-45"><a href="#cb19-45" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">end_station_id =</span> to_station_id </span>
<span id="cb19-46"><a href="#cb19-46" aria-hidden="true" tabindex="-1"></a>                   ,<span class="at">member_casual =</span> usertype)</span></code></pre></div>
<p>Next, we need to verify the data types of each column to ensure that all data is correctly formatted.</p>
<div class="sourceCode" id="cb20"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb20-1"><a href="#cb20-1" aria-hidden="true" tabindex="-1"></a><span class="fu">str</span>(q1_2019fixed)</span></code></pre></div>
<pre><code> spc_tbl_ [365,069 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
  $ ride_id           : num [1:365069] 21742443 21742444 21742445 21742446 21742447 ...
  $ started_at        : POSIXct[1:365069], format: "2019-01-01 00:04:37" "2019-01-01 00:08:13" ...
  $ ended_at          : POSIXct[1:365069], format: "2019-01-01 00:11:07" "2019-01-01 00:15:34" ...
  $ rideable_type     : num [1:365069] 2167 4386 1524 252 1170 ...
  $ tripduration      : num [1:365069] 390 441 829 1783 364 ...
  $ start_station_id  : num [1:365069] 199 44 15 123 173 98 98 211 150 268 ...
  $ start_station_name: chr [1:365069] "Wabash Ave &amp; Grand Ave" "State St &amp; Randolph St" "Racine Ave &amp; 18th St" "California Ave &amp; Milwaukee Ave" ...
  $ end_station_id    : num [1:365069] 84 624 644 176 35 49 49 142 148 141 ...
  $ end_station_name  : chr [1:365069] "Milwaukee Ave &amp; Grand Ave" "Dearborn St &amp; Van Buren St (*)" "Western Ave &amp; Fillmore St (*)" "Clark St &amp; Elm St" ...
  $ member_casual     : chr [1:365069] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
  $ gender            : chr [1:365069] "Male" "Female" "Female" "Male" ...
  $ birthyear         : num [1:365069] 1989 1990 1994 1993 1994 ...
  - attr(*, "spec")=
   .. cols(
   ..   trip_id = col_double(),
   ..   start_time = col_datetime(format = ""),
   ..   end_time = col_datetime(format = ""),
   ..   bikeid = col_double(),
   ..   tripduration = col_number(),
   ..   from_station_id = col_double(),
   ..   from_station_name = col_character(),
   ..   to_station_id = col_double(),
   ..   to_station_name = col_character(),
   ..   usertype = col_character(),
   ..   gender = col_character(),
   ..   birthyear = col_double()
   .. )
  - attr(*, "problems")=&lt;externalptr&gt;</code></pre>
  <div class="sourceCode" id="cb22"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb22-1"><a href="#cb22-1" aria-hidden="true" tabindex="-1"></a><span class="fu">str</span>(q2_2019fixed)</span></code></pre></div>
  <pre><code> spc_tbl_ [1,108,163 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
  $ ride_id           : num [1:1108163] 22178529 22178530 22178531 22178532 22178533 ...
  $ started_at        : POSIXct[1:1108163], format: "2019-04-01 00:02:22" "2019-04-01 00:03:02" ...
  $ ended_at          : POSIXct[1:1108163], format: "2019-04-01 00:09:48" "2019-04-01 00:20:30" ...
  $ rideable_type     : num [1:1108163] 6251 6226 5649 4151 3270 ...
  $ tripduration      : num [1:1108163] 446 1048 252 357 1007 ...
  $ start_station_id  : num [1:1108163] 81 317 283 26 202 420 503 260 211 211 ...
  $ start_station_name: chr [1:1108163] "Daley Center Plaza" "Wood St &amp; Taylor St" "LaSalle St &amp; Jackson Blvd" "McClurg Ct &amp; Illinois St" ...
  $ end_station_id    : num [1:1108163] 56 59 174 133 129 426 500 499 211 211 ...
  $ end_station_name  : chr [1:1108163] "Desplaines St &amp; Kinzie St" "Wabash Ave &amp; Roosevelt Rd" "Canal St &amp; Madison St" "Kingsbury St &amp; Kinzie St" ...
  $ member_casual     : chr [1:1108163] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
  $ gender            : chr [1:1108163] "Male" "Female" "Male" "Male" ...
  $ birthyear         : num [1:1108163] 1975 1984 1990 1993 1992 ...
  - attr(*, "spec")=
   .. cols(
   ..   `01 - Rental Details Rental ID` = col_double(),
   ..   `01 - Rental Details Local Start Time` = col_datetime(format = ""),
   ..   `01 - Rental Details Local End Time` = col_datetime(format = ""),
   ..   `01 - Rental Details Bike ID` = col_double(),
   ..   `01 - Rental Details Duration In Seconds Uncapped` = col_number(),
   ..   `03 - Rental Start Station ID` = col_double(),
   ..   `03 - Rental Start Station Name` = col_character(),
   ..   `02 - Rental End Station ID` = col_double(),
   ..   `02 - Rental End Station Name` = col_character(),
   ..   `User Type` = col_character(),
   ..   `Member Gender` = col_character(),
   ..   `05 - Member Details Member Birthday Year` = col_double()
   .. )
  - attr(*, "problems")=&lt;externalptr&gt;</code></pre>
  <div class="sourceCode" id="cb24"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb24-1"><a href="#cb24-1" aria-hidden="true" tabindex="-1"></a><span class="fu">str</span>(q3_2019fixed)</span></code></pre></div>
  <pre><code> spc_tbl_ [1,640,718 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
  $ ride_id           : num [1:1640718] 23479388 23479389 23479390 23479391 23479392 ...
  $ started_at        : POSIXct[1:1640718], format: "2019-07-01 00:00:27" "2019-07-01 00:01:16" ...
  $ ended_at          : POSIXct[1:1640718], format: "2019-07-01 00:20:41" "2019-07-01 00:18:44" ...
  $ rideable_type     : num [1:1640718] 3591 5353 6180 5540 6014 ...
  $ tripduration      : num [1:1640718] 1214 1048 1554 1503 1213 ...
  $ start_station_id  : num [1:1640718] 117 381 313 313 168 300 168 313 43 43 ...
  $ start_station_name: chr [1:1640718] "Wilton Ave &amp; Belmont Ave" "Western Ave &amp; Monroe St" "Lakeview Ave &amp; Fullerton Pkwy" "Lakeview Ave &amp; Fullerton Pkwy" ...
  $ end_station_id    : num [1:1640718] 497 203 144 144 62 232 62 144 195 195 ...
  $ end_station_name  : chr [1:1640718] "Kimball Ave &amp; Belmont Ave" "Western Ave &amp; 21st St" "Larrabee St &amp; Webster Ave" "Larrabee St &amp; Webster Ave" ...
  $ member_casual     : chr [1:1640718] "Subscriber" "Customer" "Customer" "Customer" ...
  $ gender            : chr [1:1640718] "Male" NA NA NA ...
  $ birthyear         : num [1:1640718] 1992 NA NA NA NA ...
  - attr(*, "spec")=
   .. cols(
   ..   trip_id = col_double(),
   ..   start_time = col_datetime(format = ""),
   ..   end_time = col_datetime(format = ""),
   ..   bikeid = col_double(),
   ..   tripduration = col_number(),
   ..   from_station_id = col_double(),
   ..   from_station_name = col_character(),
   ..   to_station_id = col_double(),
   ..   to_station_name = col_character(),
   ..   usertype = col_character(),
   ..   gender = col_character(),
   ..   birthyear = col_double()
   .. )
  - attr(*, "problems")=&lt;externalptr&gt;</code></pre>
  <div class="sourceCode" id="cb26"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb26-1"><a href="#cb26-1" aria-hidden="true" tabindex="-1"></a><span class="fu">str</span>(q4_2019fixed)</span></code></pre></div>
  <pre><code> spc_tbl_ [704,054 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
  $ ride_id           : num [1:704054] 25223640 25223641 25223642 25223643 25223644 ...
  $ started_at        : POSIXct[1:704054], format: "2019-10-01 00:01:39" "2019-10-01 00:02:16" ...
  $ ended_at          : POSIXct[1:704054], format: "2019-10-01 00:17:20" "2019-10-01 00:06:34" ...
  $ rideable_type     : num [1:704054] 2215 6328 3003 3275 5294 ...
  $ tripduration      : num [1:704054] 940 258 850 2350 1867 ...
  $ start_station_id  : num [1:704054] 20 19 84 313 210 156 84 156 156 336 ...
  $ start_station_name: chr [1:704054] "Sheffield Ave &amp; Kingsbury St" "Throop (Loomis) St &amp; Taylor St" "Milwaukee Ave &amp; Grand Ave" "Lakeview Ave &amp; Fullerton Pkwy" ...
  $ end_station_id    : num [1:704054] 309 241 199 290 382 226 142 463 463 336 ...
  $ end_station_name  : chr [1:704054] "Leavitt St &amp; Armitage Ave" "Morgan St &amp; Polk St" "Wabash Ave &amp; Grand Ave" "Kedzie Ave &amp; Palmer Ct" ...
  $ member_casual     : chr [1:704054] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
  $ gender            : chr [1:704054] "Male" "Male" "Female" "Male" ...
  $ birthyear         : num [1:704054] 1987 1998 1991 1990 1987 ...
  - attr(*, "spec")=
   .. cols(
   ..   trip_id = col_double(),
   ..   start_time = col_datetime(format = ""),
   ..   end_time = col_datetime(format = ""),
   ..   bikeid = col_double(),
   ..   tripduration = col_number(),
   ..   from_station_id = col_double(),
   ..   from_station_name = col_character(),
   ..   to_station_id = col_double(),
   ..   to_station_name = col_character(),
   ..   usertype = col_character(),
   ..   gender = col_character(),
   ..   birthyear = col_double()
   .. )
  - attr(*, "problems")=&lt;externalptr&gt;</code></pre>
  <p>We need to convert gender to characters so we can merge the tables.</p>
  <div class="sourceCode" id="cb28"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb28-1"><a href="#cb28-1" aria-hidden="true" tabindex="-1"></a>q4_2019fixed <span class="ot">&lt;-</span>  <span class="fu">mutate</span>(q4_2019fixed, <span class="at">gender =</span> <span class="fu">as.character</span>(gender)) </span>
<span id="cb28-2"><a href="#cb28-2" aria-hidden="true" tabindex="-1"></a>q3_2019fixed <span class="ot">&lt;-</span>  <span class="fu">mutate</span>(q3_2019fixed, <span class="at">gender =</span> <span class="fu">as.character</span>(gender)) </span>
<span id="cb28-3"><a href="#cb28-3" aria-hidden="true" tabindex="-1"></a>q2_2019fixed <span class="ot">&lt;-</span>  <span class="fu">mutate</span>(q2_2019fixed, <span class="at">gender =</span> <span class="fu">as.character</span>(gender))</span>
<span id="cb28-4"><a href="#cb28-4" aria-hidden="true" tabindex="-1"></a>q1_2019fixed <span class="ot">&lt;-</span>  <span class="fu">mutate</span>(q1_2019fixed, <span class="at">gender =</span> <span class="fu">as.character</span>(gender))</span></code></pre></div>
<p>Now, I will merge the dataframes into one dataframe.</p>
<div class="sourceCode" id="cb29"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb29-1"><a href="#cb29-1" aria-hidden="true" tabindex="-1"></a>trips <span class="ot">=</span> <span class="fu">bind_rows</span>(q1_2019fixed, q2_2019fixed, q3_2019fixed, q4_2019fixed)</span></code></pre></div>
<p>The next step is to clean the data.</p>
<p>We will add columns that list the day, month, day, and year of eachtrip. This will allow us to aggregate trip data for each month, day or year.</p>
<div class="sourceCode" id="cb30"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb30-1"><a href="#cb30-1" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>date <span class="ot">&lt;-</span> <span class="fu">as.Date</span>(trips<span class="sc">$</span>started_at) </span>
<span id="cb30-2"><a href="#cb30-2" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>month <span class="ot">&lt;-</span> <span class="fu">format</span>(<span class="fu">as.Date</span>(trips<span class="sc">$</span>date), <span class="st">"%m"</span>)</span>
<span id="cb30-3"><a href="#cb30-3" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>day <span class="ot">&lt;-</span> <span class="fu">format</span>(<span class="fu">as.Date</span>(trips<span class="sc">$</span>date), <span class="st">"%d"</span>)</span>
<span id="cb30-4"><a href="#cb30-4" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>year <span class="ot">&lt;-</span> <span class="fu">format</span>(<span class="fu">as.Date</span>(trips<span class="sc">$</span>date), <span class="st">"%Y"</span>)</span>
<span id="cb30-5"><a href="#cb30-5" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>day_of_week <span class="ot">&lt;-</span> <span class="fu">format</span>(<span class="fu">as.Date</span>(trips<span class="sc">$</span>date), <span class="st">"%A"</span>)</span>
<span id="cb30-6"><a href="#cb30-6" aria-hidden="true" tabindex="-1"></a><span class="fu">colnames</span>(trips)</span></code></pre></div>
<pre><code>  [1] "ride_id"            "started_at"         "ended_at"          
  [4] "rideable_type"      "tripduration"       "start_station_id"  
  [7] "start_station_name" "end_station_id"     "end_station_name"  
 [10] "member_casual"      "gender"             "birthyear"         
 [13] "date"               "month"              "day"               
 [16] "year"               "day_of_week"</code></pre>
 <p>Now, we will create a column for trip duration by calculating the time difference between the trip's start and end times.</p>

 <div class="sourceCode" id="cb32"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb32-1"><a href="#cb32-1" aria-hidden="true" tabindex="-1"></a>trips<span class="sc">$</span>ride_length <span class="ot">=</span> <span class="fu">difftime</span>(trips<span class="sc">$</span>ended_at,trips<span class="sc">$</span>started_at,<span class="sc"> </span>units="mins")</span></code></pre></div>
 <p>There is some "bad" data to remove, where ride_length is negative due to bikes being taken out for maintenance or quality checks. We will create a new dataframe that excludes these trips with negative trip lengths.</p>
<div class="sourceCode" id="cb33"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb33-1"><a href="#cb33-1" aria-hidden="true" tabindex="-1"></a>trip_data_clean <span class="ot">&lt;-</span> trips[<span class="sc">!</span>(trips<span class="sc">$</span>ride_length <span class="sc">&lt;=</span> <span class="dv">0</span>),]</span>
<span id="cb33-2"><a href="#cb33-2" aria-hidden="true" tabindex="-1"></a><span class="fu">glimpse</span>(trip_data_clean)</span></code></pre></div>
<pre><code> Rows: 4,561,085
Columns: 18
$ ride_id            <dbl> 22178529, 22178530, 22178531, 22178532, 22178533, …
$ started_at         <dttm> 2019-04-01 00:02:22, 2019-04-01 00:03:02, 2019-04…
$ ended_at           <dttm> 2019-04-01 00:09:48, 2019-04-01 00:20:30, 2019-04…
$ rideable_type      <dbl> 6251, 6226, 5649, 4151, 3270, 3123, 6418, 4513, 32…
$ tripduration       <dbl> 446, 1048, 252, 357, 1007, 257, 548, 383, 2137, 21…
$ start_station_id   <dbl> 81, 317, 283, 26, 202, 420, 503, 260, 211, 211, 30…
$ start_station_name <chr> "Daley Center Plaza", "Wood St & Taylor St", "LaSa…
$ end_station_id     <dbl> 56, 59, 174, 133, 129, 426, 500, 499, 211, 211, 23…
$ end_station_name   <chr> "Desplaines St & Kinzie St", "Wabash Ave & Rooseve…
$ member_casual      <chr> "Subscriber", "Subscriber", "Subscriber", "Subscri…
$ gender             <chr> "Male", "Female", "Male", "Male", "Male", "Male", …
$ birthyear          <dbl> 1975, 1984, 1990, 1993, 1992, 1999, 1969, 1991, NA…
$ date               <date> 2019-04-01, 2019-04-01, 2019-04-01, 2019-04-01, 2…
$ month              <chr> "04", "04", "04", "04", "04", "04", "04", "04", "0…
$ day                <chr> "01", "01", "01", "01", "01", "01", "01", "01", "0…
$ year               <chr> "2019", "2019", "2019", "2019", "2019", "2019", "2…
$ day_of_week        <chr> "Monday", "Monday", "Monday", "Monday", "Monday", …
$ ride_length        <drtn> 7.433333 mins, 17.466667 mins, 4.200000 mins, 5.9…</code></pre>
 <h2 id="analyze">ANALYZE</h2>
 <p>We will now conduct a descriptive analysis to identify patterns between Customers and Subscribers. Before starting the analysis, it's important to first review the basic descriptive statistics of the data.</p>
<div class="sourceCode" id="cb35"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb35-1"><a href="#cb35-1" aria-hidden="true" tabindex="-1"></a><span class="fu">mean</span>(trip_data_clean<span class="sc">$</span>ride_length)</span>
<span id="cb35-2"><a href="#cb35-2" aria-hidden="true" tabindex="-1"></a><span class="fu">median</span>(trip_data_clean<span class="sc">$</span>ride_length) </span>
<span id="cb35-3"><a href="#cb35-3" aria-hidden="true" tabindex="-1"></a><span class="fu">max</span>(trip_data_clean<span class="sc">$</span>ride_length) </span>
<span id="cb35-4"><a href="#cb35-4" aria-hidden="true" tabindex="-1"></a><span class="fu">min</span>(trip_data_clean<span class="sc">$</span>ride_length)</span></code></pre></div>
<pre><code>     
> <b>mean(trip_data_clean$ride_length)</b>
Time difference of 24.25457 mins
> <b>median(trip_data_clean$ride_length</b>
Time difference of 12.28333 mins
> <b>max(trip_data_clean$ride_length)</b>
Time difference of 150943.9 mins
> <b>min(trip_data_clean$ride_length)</b>
Time difference of 1.016667 mins</code></pre>
     <p>First, we'll compare Customer and Subscriber trip stats.</p>
     <div class="sourceCode" id="cb37"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb37-1"><a href="#cb37-1" aria-hidden="true" tabindex="-1"></a><span class="fu">aggregate</span>(trip_data_clean<span class="sc">$</span>ride_length <span class="sc">,</span> by = list(trip_data_clean<span class="sc">$</span>member_casual),<span class="at">FUN =</span> mean)</span>
<span id="cb37-2"><a href="#cb37-2" aria-hidden="true" tabindex="-1"></a><span class="fu">aggregate</span>(trip_data_clean<span class="sc">$</span>ride_length <span class="sc">,</span> by = list(trip_data_clean<span class="sc">$</span>member_casual), <span class="at">FUN =</span> median)</span>
<span id="cb37-3"><a href="#cb37-3" aria-hidden="true" tabindex="-1"></a><span class="fu">aggregate</span>(trip_data_clean<span class="sc">$</span>ride_length <span class="sc">,</span> by = list(trip_data_clean<span class="sc">$</span>member_casual), <span class="at">FUN =</span> max)</span>
<span id="cb37-4"><a href="#cb37-4" aria-hidden="true" tabindex="-1"></a><span class="fu">aggregate</span>(trip_data_clean<span class="sc">$</span>ride_length <span class="sc">,</span> by = list (trip_data_clean<span class="sc">$</span>member_casual), <span class="at">FUN =</span> min)</span></code></pre></div>
<p>Before continuing, arrange the day_of_week column in the correct order.</p>
<div class="sourceCode" id="cb38"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb38-1"><a href="#cb38-1" aria-hidden="true" tabindex="-1"></a>trip_data_clean<span class="sc">$</span>day_of_week <span class="ot">&lt;-</span> <span class="fu">ordered</span>(trip_data_clean<span class="sc">$</span>day_of_week, <span class="at">levels=</span><span class="fu">c</span>(<span class="st">"Sunday"</span>, <span class="st">"Monday"</span>, <span class="st">"Tuesday"</span>, <span class="st">"Wednesday"</span>, <span class="st">"Thursday"</span>, <span class="st">"Friday"</span>, <span class="st">"Saturday"</span>))</span></code></pre></div>
<p>Next, we will check the average ride time per day and the total number of trips for Customer and Subscriber</p>
<div class="sourceCode" id="cb39"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb39-1"><a href="#cb39-1" aria-hidden="true" tabindex="-1"></a>plot <span class="ot">&lt;-</span> trip_data_clean <span class="sc">%&gt;%</span> </span>
<span id="cb39-2"><a href="#cb39-2" aria-hidden="true" tabindex="-1"></a>  <span class="fu">group_by</span>(member_casual, day_of_week) <span class="sc">%&gt;%</span>  <span class="co">#groups by member_casual</span></span>
<span id="cb39-3"><a href="#cb39-3" aria-hidden="true" tabindex="-1"></a>  <span class="fu">summarise</span>(<span class="at">number_of_rides =</span> <span class="fu">n</span>() <span class="co">#calculates the number of rides and average duration </span></span>
<span id="cb39-4"><a href="#cb39-4" aria-hidden="true" tabindex="-1"></a>  ,<span class="at">average_ride_length =</span> <span class="fu">mean</span>(ride_length),<span class="at">.groups=</span><span class="st">"drop"</span>) <span class="sc">%&gt;%</span> <span class="co"># calculates the average duration</span></span>
<span id="cb39-5"><a href="#cb39-5" aria-hidden="true" tabindex="-1"></a>  <span class="fu">arrange</span>(member_casual, day_of_week) <span class="co">#sort</span></span></code></pre></div>
<h2 id="share">Share</h2>
<p>We'll analyze the data through visualizations first, which will help us identify key insights and effectively present our findings to the marketing department and other stakeholders.</p>
<div class="sourceCode" id="cb40"><pre class="sourceCode r"><code class="sourceCode r"><span id="cb40-1"><a href="#cb40-1" aria-hidden="true" tabindex="-1"></a> <span class="fu">ggplot</span>(plot,<span class="fu">aes</span>(<span class="at">x =</span> day_of_week, <span class="at">y =</span> number_of_rides, <span class="at">fill =</span> member_casual)) <span class="sc">+</span></span>
<span id="cb40-2"><a href="#cb40-2" aria-hidden="true" tabindex="-1"></a> <span class="fu">labs</span>(<span class="at">title =</span><span class="st">"Total rides of Members and Casual riders Vs. Day of the week"</span>) <span class="sc">+</span></span>
<span id="cb40-3"><a href="#cb40-3" aria-hidden="true" tabindex="-1"></a> <span class="fu">geom_col</span>(<span class="at">width=</span><span class="fl">0.5</span>, <span class="at">position =</span> <span class="fu">position_dodge</span>(<span class="at">width=</span><span class="fl">0.5</span>))<span class="sc">+</span></span>
<span id="cb40-4"><a href="#cb40-4" aria-hidden="true" tabindex="-1"></a> <span class="fu">scale_y_continuous</span>(<span class="at">labels =</span> <span class="cf">function</span>(x) <span class="fu">format</span>(x, <span class="at">scientific =</span> <span class="cn">FALSE</span>))</span></code></pre></div>
<p align="center">
  <img src="/IMG/1.png">
</p>
<p>Based on the chart above, the Subscriber group has the highest number of rides during weekdays.</p
