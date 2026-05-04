1) For the weather data, specify the Spark commands to create an RDD that has the max temperatures per year for each weather station

```python
inpRDD.filter(lambda x: "STATION_NAME" not in x) \
	.map(lambda x: x.split(",")) \
	.map(lambda x: (str(x[1])+","+str(x[2])[:4],int(x[4]))) \
	.groupByKey().mapValues(max)
```

```
[('STAVENISSE NL,1981', '-9999'), ('STAVENISSE NL,1982', '-9999'), ('STAVENISSE NL,1983', '-9999'), ('STAVENISSE NL,1984', '-9999'), ('STAVENISSE NL,1988', '-9999'), ('STAVENISSE NL,1997', '-9999'), ('STAVENISSE NL,1998', '-9999'), ('STAVENISSE NL,2002', '-9999'), ('STAVENISSE NL,2004', '-9999'), ('STAVENISSE NL,2005', '-9999'), ('STAVENISSE NL,2011', '-9999'), ('STAVENISSE NL,2012', '-9999'), ('STAVENISSE NL,2013', '-9999'), ('DE BILT 1 NL,1983', '99'), ('DE BILT 1 NL,1984', '99'), ('DE BILT 1 NL,1986', '99'), ('DE BILT 1 NL,1989', '99'), ('DE BILT 1 NL,1990', '99'), ('DE BILT 1 NL,1994', '98'), ('DE BILT 1 NL,1999', '99')]
```


2) Create an RDD that has the max temperatures per year for only the Barcelona airport (BARCELONA AEROPUERTO)


```python
inpRDD.filter(lambda x: "BARCELONA AEROPUERTO" in x) \
	.map(lambda x: x.split(",")) \
	.map(lambda x: (str(x[2])[:4],int(x[4]))) \
	.groupByKey().mapValues(max)
```

res: 
```
[('1980', '98'), ('1981', '98'), ('1985', '96'), ('1993', '96'), ('1995', '84'), ('2001', '94'), ('2003', '94'), ('2007', '91'), ('2008', '99'), ('2011', '98'), ('2012', '94'), ('2014', '90'), ('1982', '98'), ('1983', '96'), ('1984', '92'), ('1986', '96'), ('1987', '95'), ('1990', '96'), ('1992', '99'), ('1997', '98'), ('1999', '94'), ('2005', '97'), ('2006', '99'), ('2010', '99'), ('2013', '98'), ('1988', '98'), ('1989', '98'), ('1991', '98'), ('1994', '98'), ('1996', '95'), ('1998', '94'), ('2000', '97'), ('2002', '96'), ('2004', '96'), ('2009', '99')]
```
bonus) From #2, what is the average of all the max temperatures for the Bercelona
airport (BARCELONA AEROPUERTO)

```python
inpRDD.filter(lambda x: "BARCELONA AEROPUERTO" in x and "-9999" not in x) \
	.map(lambda x: x.split(",")) \
	.map(lambda x: int(x[4])) \
	.mean()
```

res: 
```
204.5444315737208
```
