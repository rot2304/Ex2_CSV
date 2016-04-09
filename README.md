# דו"ח מסכם- קובץ נדל"ן בארה"ב

##תיאור הנתונים- מידע על הקובץ:
* מקובץ הטרנזקציות של הנדל"ן בסקרמנטו הוא רשימה של 985 טרנזקציות נדל"ן באיזור סקרמנטו שדווחו לאורך תקופה של חמישה ימים כפי שדווח ב Sacramento Bee
* לקובץ יש מידע על הכתובות שניתן וגם מיקומי מיקום (קוארדינטות אורך ורוחב).
* הקובץ מכיל את העמודות הבאות:
רחוב, עיר, מיקוד, מדינה, חדרי שינה, מקלחות, מטר מרובע, סוג, תאריך המכירה, מחיר, קו אורך, קו רוחב

##תיעוד הקוד לקריאת הרשומות מקובץ הנתונים:
השמת כתובת הוקבץ במשתנה והורדת הקובץ לR:
```{r}
fileUrl = http://samplecsvs.s3.amazonaws.com/Sacramentorealestatetransactions.csv 
download.file(fileUrl,"./R/Work1/data.csv",method="curl")
mydata = read.csv("data.csv",colClasses = c("character","factor","numeric","character","numeric","numeric","numeric","character","character","numeric","numeric","numeric"))
```

##ניתוח הנתונים:
###גרפים:
####2D boxplot:
```{r}
boxplot(mydata$price~mydata$city,col="blue",las=2,mar = c(10,4,4,2) + 0.1)
```
![](https://cloud.githubusercontent.com/assets/17852872/14242690/0dc4a75e-fa5a-11e5-81e7-a87f9255ea31.png)

זהו בוקס פלוט בו ציר האיקס מתאר את הערים בארה"ב בהן מתבצעות עסקאות נדל"ן  וציר הוואי מתאר את טווח המחירים. ניתן לראות כי ברוב המקומות ההמחירים נעים סביב אותו ציר , אך ישנם מעט מקומות קיצוניים מבחינת מחירי הנדל"ן , ננסה למצוא את הסיבה לכך בהמשך.

####Scatterplot:
```{r}
with(mydata,plot(latitude,price,col=city))
```
![](https://cloud.githubusercontent.com/assets/17852872/14242801/ad454644-fa5a-11e5-830f-ddbe2234e428.jpg)

זהו תרשים פיזור אשר  בו בציר האנכי (משתנה הווי)- המשתנה המסביר. 
בציר האופקי (משתנה ה-איקס) המשתנה אותו אנו חוקרים האם ישנו קשר בין לבין המשתנה המסביר (המוסבר).
בתרשים שלנו ניתן לראות את הפיזור של הערים ע"פ קוארדינטת קו רוחב ומחירים, ע"פ הצבע כמובן.
כלומר, כל נקודה בגרף הפיזור מייצגת רשומה מסוימת מקובץ הנתונים. נקודות אשר באותו הצבע מייצגות רשומות אשר להן אותה העיר. ועל כן ניתן לראות בתרשים הפיזור את פיזור הערים ע"פ מחירים וקווי הרוחב.

קל לראות ע"פ התבוננות בגרף הפיזור כי נקודות אשר באותה הצבע מרוכזות באותו האיזור, כלומר נכסים אשר באותה עיר טווח המחירים שלהם נע סביב אותו איזור והגיוני כמובן שאם הם באותה העיר הם גם נעים סביב אותו טווח שלקו רוחב.
עם זאת, הקשר בין מחיר הנכס לבין העיר איננו מובהק לחלוטין וישיר לגמרי שכן ישנם הרבה יוצאים מן הכלל אשר מתפזרים לאיזורים שונים בגרף ולכן הקשר לא כזה חזק וסביר להניח שישנם מאפיינים אחרים אשר להם השפעה ישירה יותר על המחיר.


באמצעות הפקודות הבאות ניתן לייצר אותו הגרף בתוספת שמות הערים. כלומר לצד כל נקודה שמייצגת רשומה ניתן לראות בדיוק אותה עיר שבה:
```{r}
plot(mydata$latitude,mydata$price,col=mydata$city) 
text(mydata$latitude,mydata$price, labels=cities, cex= 0.7)
```
![](https://cloud.githubusercontent.com/assets/17852872/14242802/ad45aea4-fa5a-11e5-9ff8-526bf3c04c43.jpg)
אם ע"פ הגרף הקודם הצלחנו לראות שנכסים שבאותה העיר לעיתים מתרכזים סביב אותו טווח מחירים וסביב אותו קו רוחב, כעת בגרף זה אנחנו כבר רואים את מגמות המחירים ע"פ הערים עצמן. ניתן לראות למשל שבערים אלק גרוב פלייסרבל (ועוד) המחירים הגבוהים ביותר ובערים  גאלט רינווד (ועוד) המחירים הנמוכים ביותר.
ניתן גם לראות שרוב הערים שבהם המחירים גבוהים מהרגיל הם נמצאות בקווי הרוחב בעלי הערכים הגבוהים יותר. עם זאת, גם כעת ניתן לראות שישנם ערים שמתפרשות לאורך כל התרשים ושוב ישנם הרבה יוצאים מהכלל ולכן אנחנו לא יכולים להגיד שהעיר משפיעה על המחיר באופן מוחלט ובאופן גורף. 
בשלב הבא כאשר ניצור מפה ננסה להסיק מסקנות יותר מוחשיות לגבי הקשר בין המיקום הגיאוגרפי של הערים לבין המחירים.

###יצירת מפה מנתוני המקום:
```{r}
map <- get_map(location = c(lon = mean(mydata$longitude), lat = mean(mydata$latitude)), zoom = 10, maptype = "satellite", scale = 2)
ggmap(map) + geom_point(data = mydata, aes(x = longitude, y = latitude, colour = ifelse(price>500000,F,T), alpha = 0.1), size = 2, shape = 21) + guides(fill=TRUE, alpha=FALSE, size=FALSE
```
![](https://cloud.githubusercontent.com/assets/17852872/14242799/ad44811e-fa5a-11e5-8e2a-6eec5d036965.jpg)

כפי שהתבקשנו, הקובץ מכיל נתוני מיקום שניתן למקם על מפה ומהם יצרנו מפה שנוצרה ע"פ נתוני מיקום אלו (קווי רוחב וגובה). אפשר להגיד שניתן לראות בצורה מאוד רופפת שהדירות בצפון יותר יקרות מהדרום אבל פעם נוספת הדבר מאוד לא מובהק וישנם המון יוצאים מן הכלל. כנ"ל לגבי הדירות שבמערב לעומת המזרח. בהמשך ננסה לחקור מאפיינים שלהם השפעה יותר חזקה על המחיר מאשר עיר ואיזור

###ניתוח חשיבות מאפיינים שונים:
נשתמש בחבילת caret אשר מספקת כלים שבאופן אוטומטי מדווחים על הרלוונטיות והחשיבות של מאפיינים  בנתונים ואף לבחור את המאפיינים הכי חשובים עבורנו.
```{r}
set.seed(nrow(dataset))
control <- trainControl(method="repeatedcv", number=10, repeats=3)
dataset$price_discrete <-as.factor(dataset$price_discrete)
dataset$city <-as.numeric(dataset$city)
dataset$city <-as.factor(dataset city)
x<- dataset[,c("city","sq__ft" )]
y<- dataset$price_discrete
model <- train(x, y, method="lvq", preProcess="scale", trControl=control)
importance <- varImp(model, scale=FALSE)
plot(importance)
```

![](https://cloud.githubusercontent.com/assets/17852872/14242800/ad44a8ba-fa5a-11e5-9b4a-97a68dbd6b23.png)

ע"מ להפיק את חשיבות המאפיינים היה לבצע עלינו עבודה מקדמת בהכנת הנתונים ע"מ שניתן יהיה להפעיל עליהם את פונקציות ניתוח החשיבות.
הכנת הנתונים לצורך כך כללה את הפעולות הבאות:

* 	הוספנו לקובץ שלנו עמודה אשר נקראת:
 price_discrete 
אשר יכולה לקבל את הערכים 0,1 כאשר 0 יקבלו רשומות שמחירי הדירות שלהם קטנים מ500,000 ו1 יקבלו דירות אשר מחירי הדירות שלהם גדולים מ500,000. פעולה זו הכרחית מכיוון שאימון מודל חשיבות המאפיינים אינו יודע לאמן ערכים לגבי מסווגים לא בדידים.

*	הפכנו את ערכי העמודה city לנומריים מכיוון שלא ניתן לבצע חשיבות מאפיינים על עמודות מסוג string אלא על שדות נומריים בלבד.
