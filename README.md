# ד"וח מסכם לקובץ נדל:ן בארה"ב

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


