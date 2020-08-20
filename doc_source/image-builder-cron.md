# Use cron expressions in EC2 Image Builder<a name="image-builder-cron"></a>

Cron expressions for EC2 Image Builder have five required fields: Minutes, hours, day of the month, month, and day of the week\. Fields are separated by a space\.


**Cron expression examples**  

| Minutes | Hours | Day of the month | Month | Day of the week | Meaning | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | \* | \* | * | Run at 10:00 am \(UTC\) every day | 
| 15 | 12 | \* | \* | * | Run at 12:15 PM \(UTC\) every day | 
| 0 | 18 | ? | \* | MON\-FRI | Run at 6:00 PM \(UTC\) every Monday through Friday | 
| 0 | 8 | 1 | \* | * | Run at 8:00 AM \(UTC\) every 1st day of the month | 
| 0 | 10 | \* | \* | TUE\#2 | Runs the second Tuesday of every month at 10:00am \(UTC\)\. | 

**Supported Values**  
The following table shows supported values for required cron entries\.


**Supported values for cron expressions**  

| Field | Values | Wildcards | 
| --- | --- | --- | 
| Minutes | 0\-59 | , \- \* | 
| Hours | 0\-23 | , \- \* / | 
| Day of month | 1\-31 | , \- \* ? / L | 
| Month | 1\-12 or JAN\-DEC | , \- \* / | 
| Day of week | 1\-7 or SUN\-SAT | , \- \* ? / L \# | 

**Note**  
You cannot specify a value in the day\-of\-the\-month and in the day\-of\-the\-week fields in the same cron expression\. If you specify a value in one of the fields, you must use a `?` \(question mark\) in the other field\.

**Wildcards**  
The following table shows the wildcard values that cron expressions support in EC2 Image Builder \.

**Important**  
Wildcards are not permitted in the minute or hour field\. You must specify a fixed hour and minute to run your builds \(for example, `0 12`\)\.


**Supported wildcards for cron expressions**  

| Wildcard | Description | 
| --- | --- | 
| , | The , \(comma\) wildcard includes additional values\. In the Month field, JAN,FEB,MAR includes January, February, and March\. | 
| \- | The \- \(dash\) wildcard specifies ranges\. In the Day field, 1\-15 includes days 1 through 15 of the specified month\. | 
| \* | The \* \(asterisk\) wildcard includes all values in the field\.  | 
| / | The / \(forward slash\) wildcard specifies increments\. | 
| ? | The ? \(question mark\) wildcard specifies one or the other\. In the day\-of\-the\-month field, you can enter 7 and, if it does not matter what day of the week the 7th is, you can enter ? in the day\-of\-the\-week field\. | 
| L | The L wildcard in the day\-of\-the\-month or day\-of\-the\-week fields specifies the last day of the month or week\. | 
| \# | The \# \(hash\) is allowed only for the day\-of\-the\-week field and must be followed by a number between 1 and 5\. It specifies the day of the week in a given month, such as the second Friday of each month\. For example, TUE\#2 specifies the second Tuesday of each month\. | 
