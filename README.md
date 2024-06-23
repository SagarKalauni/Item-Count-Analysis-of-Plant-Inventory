
Data © PSGC

Report by SagarKalauni

Premision for github by Josh Hangene [Controller]

**ABC Analysis of Plant Inventory:**

### Background of How I Got This Project

After finishing my analysis of the plant inventory using ABC analysis, I showed it to Justin, the Warehouse Supervisor. He really liked the dashboard and said it would help him work more efficiently and save time. As a data analyst, it's very satisfying to see your work make a real difference in helping the company grow and making the employees' jobs easier by saving time. Justin even asked for a new dashboard to track item counts. 

The company needs to count inventory items at specific intervals based on their category to ensure they never run out when needed. In the past, they used to do this manually in Excel, which was difficult, time-consuming, and not very organized or systematic.
### About the Project

At the End of every year ABC analysis is done and the new category of the item was updated in the maximo. On the basis the category of the item it has cetain frequency of the time on which need need to be counted.

For this project even stakeholder does not have idea what they actually want in the dashboard so this dashboard was intreasting to built.

Depending upon the items they have the certain frequency of days on whcih it need to be recounted. And they are as follows

A items -- 270 days
B items -- 365 days
C items -- 730 days.

As obvious more important items to the company need to be counted more frequently then others.

So idea for this project in my mind was I will look at the last physical count date for each item and add the frequency number to that particular item on the basis of its category and find new count date.

Once I got new count date then I will subtranct that day from Today() and look for remaining days.

### Data Cleaning

For this project, most of the data cleaning steps are similar to those for the ABC analysis. However, there's a key difference: for ABC analysis, each item type is analyzed only once, so items must be distinct. In contrast, for inventory counting, items can be duplicated because they might be stored in different locations within the warehouse, each with its own physical count date.

When I mention 'location,' it's represented by 'Binnum' in our dataset. We have two types: 'Default Binnum' and 'Binnum,' which indicate specific locations inside the warehouse.

During data cleaning, I added two new columns, 'Binnum' from the 'InvTrans' table and 'Inventory' table, to facilitate our analysis.

1. **Filtering Power Plant Data:**
   - Initially, I filtered the dataset to include only rows relevant to the power plant.

2. **Average Cost Calculation:**
   - To determine the average cost for each inventory item, I merged the dataset with the InvCost table and extracted the 'avgcost' column.

3. **Exclusion of Absolute Status Items:**
   - Items marked with an 'absolute' status were deemed irrelevant, so I removed all corresponding rows.

4. **Exclusion Based on Maximo Category and Null Average Cost:**
   - Certain items, such as capitalized items, were excluded based on their current Maximo category (N) and a null average cost. This step involved consolidating relevant columns to facilitate efficient filtering.

5. **Inclusion Based on Current Balance:**
   - Items categorized as 'N' but with existing current balances required separate handling to ensure accurate categorization. I achieved this by merging the dataset with the InvBalance table and incorporating the 'curbalance' column.

6. **Removal of Non-Stock Items with Zero Current Balance:**
   - Items classified under non-stock categories with a current balance of zero were excluded from further analysis. This step addressed inventory items located externally with vendors, emphasizing items currently in PSGC's possession.

7. **Added Binnum column to the table:**
   - The table was mearged with the InvTrans table and the Binnum column was added to the data table.
   
 

Below is the M-Code with comments detailing each step for clarity and reproducibility:

![1](https://github.com/SagarKalauni/ABC-Analysis-of-Plant-Inventory/assets/141047160/acca8a6a-51dc-41ec-be3c-ddac83c24cf7)

![2](https://github.com/SagarKalauni/ABC-Analysis-of-Plant-Inventory/assets/141047160/9bc73ef3-4d40-4d37-be2a-c42cba2ecfc7)


One all the basic data cleaning is done then I started modeling the data Tables

### Modeling

All the data table used for this analysis are same as the ABC analysis.
InvBalance
InvCost
Inventory
Inventory(Z-Commodities)
InvReserve
InvStatus
InvTrans
items
Stagemat

Now I started creating a data model after looking a data and observing all the data table for a while. My data model looks like below:

![Screenshot 2024-06-22 150254](https://github.com/SagarKalauni/ABC-Analysis-of-Plant-Inventory/assets/141047160/3bfc08ea-c0ed-447b-abd6-45860826118e)

## Steps Applied for Calculation



1. **New Physical Count Date Calculation**: I determined a new physical count date for each item by adding 270, 365, or 730 days to its last physical count date, based on its category.

![Screenshot 2024-06-23 130532](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/ba48f2d9-d817-4768-925c-d7819ee9ab4e)


2. **Remaining Days Calculation**: Using today's date and the new physical count date, I calculated how many days remain until the next count. This could be a positive number (future count), zero (count due today), or negative (past due count).

![Screenshot 2024-06-23 130557](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/914a7457-6e4f-4778-8381-cb08a2ae1710)

3. **Remark Classification**:
   - If Remaining days > 7: Items are due for a count more than a week from now.
   - If Remaining days = 0 to 7: Items need to be counted within this week.
   - If Remaining days = 0: Items are due for count today.
   - If Remaining days < 0: Items are past their count due date.

   ![Screenshot 2024-06-23 130619](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/b1dc650a-abf6-4b36-996a-7e979f4229e8)

4. **Total Item Count**: Considering that items can be in different locations, I also calculated the total count for each specific item.

![Screenshot 2024-06-23 130642](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/3904c294-d4e0-4634-bdd5-0a3d953f470c)

6. **Dynamic Item Count Display**: To address the delay in Maximo updates, I created a new remark that indicates whether an item was counted yesterday.

![Screenshot 2024-06-23 130713](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/3da39fd9-65c2-43d3-b53d-35817c2a6352)

7. **Last Week's Item Count**: Justin suggested displaying how many items were counted in the last week, so I added this remark to the dashboard as well.

![Screenshot 2024-06-23 130659](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/624cc3a0-8891-4f1b-9389-52c9841df696)
y/assets/141047160/3da39fd9-65c2-43d3-b53d-35817c2a6352)

These calculations and remarks were implemented as new columns in Power BI for easy filtering using slicers, rather than as DAX measures. This setup allows for efficient filtering with just one click.

I not only highlighted which items need counting, but also provided a comprehensive summary. This included the number of items needing counting today and by the end of the week, the count from last week, the total count so far, and the percentage for each category.

Total Itmes Not counted till today
![Screenshot 2024-06-23 131102](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/051c1404-afaa-4331-b4d3-e30771dda111)

No of items counted last week
![Screenshot 2024-06-23 130735](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/f92ff73c-5dcc-4269-b65b-6d0fcd697aa3)

No of items counted last day (yesterday)
![Screenshot 2024-06-23 130745](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/faa1cd86-0e1f-4970-89d3-63c081821074)

Item need counting today i.e due date is today
![Screenshot 2024-06-23 130755](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/cdd4df64-9c28-4195-b114-461fb3f8e752)

No of Items need counting by this week
![Screenshot 2024-06-23 130803](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/d026a620-f68b-4a6d-936b-0564512033a8)

No of items passed due date as of today (dynamic)
![Screenshot 2024-06-23 130852](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/c88a5d1a-0b7e-4ce9-bb97-58a0de17ba1f)

for todays date
![Screenshot 2024-06-23 130912](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/cd2da219-0ec4-4330-881a-3214280ad03f)

% of count of items that were counted last week
![Screenshot 2024-06-23 130924](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/35d2d37f-7eb0-4c87-bf02-c8ddea38bcbb)

% of count of items that were counted last day i.e yesterday
![Screenshot 2024-06-23 130945](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/36b0aa8b-bac4-4314-aefb-fa18989425cd)

% of count of items that need counting today i.e item with due date today
![Screenshot 2024-06-23 130957](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/1d0f1e92-fa9b-4746-9aba-84885808a12b)

% of count of total items counted as of today
![Screenshot 2024-06-23 131010](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/f7559465-a9c3-4e02-8070-e42c3a6669bb)

% of count of total items not counted as of today
![Screenshot 2024-06-23 131021](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/e1ddfc46-992e-4b65-a4d5-8ae86c205e79)

total items that go through Item count analysis (because item count analysis is only done for the items with category ABC currently in maximo)
![Screenshot 2024-06-23 131038](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/cd0d37a1-d7ee-4736-b9ba-1a93d5ccf374)


Total items counted as of today 
![Screenshot 2024-06-23 131051](https://github.com/SagarKalauni/Item-Count-Analysis-of-Plant-Inventory/assets/141047160/f48406c4-459c-45c6-a0ee-628232223719)



This dynamic dashboard allows Justin to easily see which items need counting soon, making inventory management efficient and organized. Personally, The project's ongoing impact and utility in daily operations are particularly fulfilling for me as a data analyst, knowing it contributes tangibly to organizational efficiency.



