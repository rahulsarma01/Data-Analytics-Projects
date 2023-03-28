# Lemonade Stand Startup Analysis using Excel 

**Scenario**: 
- create sensitivity tables to see how different price changes affect our startup business scenario
- use [goal seek](https://corporatefinanceinstitute.com/resources/excel/goal-seek-excel-guide/) and [solver](https://www.geeksforgeeks.org/how-to-use-solver-in-excel/) to see how much we need to sell to reach our financial goals

Example data from our lemonade stand:

| Feature | Description |
| --- | --- |
| Units sold | Quantity of lemonade sold  |
| Price per unit | Price  per lemonade |
| Cost per unit | Cost for us to make one lemonade |
| G&A | General and Administrative costs(rents, salaries) |
| Tax rate |  |


![14](https://user-images.githubusercontent.com/113444489/228234757-5f583d79-3b89-4e71-ba0f-7fb6b03b1f55.png)


Objective: we  want to get 'Net Income' to boost all the way to 100 000:
- One way that we can do that is by changing  the quantity sold.
- Another way is by using goal seek(what-if analysis)

![12](https://user-images.githubusercontent.com/113444489/228229643-4a5a7b70-05df-428e-bb82-0ee5b6adaded.png)

One problem with our analysis is that in order to make Net Income 100,000 we have to sell 60,703 lemonades.

- 60 000 units seems like quite a lot of work for us and we want to limit that to around 25 000 as  anything more is probably just not worth it
- Other variable that we could tweak is Price. We are going to assume max price to be 9.99.

For this task we will use Solver where we can tweak more than one variables.

![13](https://user-images.githubusercontent.com/113444489/228233189-456a9ac7-7d4d-4cea-8ce3-999b2c69ad04.png)

*This is the end of our analysis. Thank you for going through my analysis.*
