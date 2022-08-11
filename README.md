![3cbe7aaa7e6a41da16a0169e2e18168](https://user-images.githubusercontent.com/97984680/181851767-cbaff7ab-2022-4e31-a8e2-c2fd609c8c26.png)


Aim to analyze NBA player's performance based on their colleges

# Dataset Introduction

Columns(24)

Time span: NBA Pick from 1989-2021

Data from: https://www.basketball-reference.com

Link for this work in my kaggle:https://www.kaggle.com/code/eames07/college-students-nba-performance-analysis

Note: I highly recommend opening the link above and checking my work since animation and hover data can't be shown in the ReadMe file.


# Rank colleges by player's total minutes in NBA

```python
df1=draft.groupby(["college"]).sum().sort_values(by=["minutes_played"],ascending=False)
df2=df1[["games","minutes_played","points","total_rebounds","assists"]]
df2=df2.reset_index()

df2=df2.reset_index()
df3=df2[["index","college","minutes_played"]]
df3.columns=["college_rank","college","minutes_played"]
df4=df3.merge(draft,left_on="college",right_on="college",suffixes=('_left', '_right'))

df3.head(30)
```
![b43231e2c07c2ac9d6f8558d05906e6](https://user-images.githubusercontent.com/97984680/181853071-712bebcd-769c-41db-84a4-d4e82f8c53d1.png)

# NBA Draft Analysis (By College)
```python
df0 = df4[df4["games"] > 0]

sorted_df = df0.sort_values(by = "college_rank", ascending = True)

fig = px.scatter(sorted_df, x = "overall_pick", y = "points_per_game",
                    range_x = (0, 60), range_y = (0, 30),
                    hover_data = ["college"],
                    hover_name="player",
                    animation_frame = "college_rank", range_color = (0, 25),
                    color = "points_per_game", color_continuous_scale = "Blues")
                    #title = "Points Per Game Overall Pick 1-60 Animation by Year and Games")

fig.update_traces(marker = dict(size = 5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```
![2254114e748f12aea4f31a3cfd5c7dc](https://user-images.githubusercontent.com/97984680/181853179-5ad9f8e4-2708-4242-974b-46329596e115.png)

# NBA Players' Cumulative Stats Analysis (By College)

## Total NBA Minutes, Total NBA Games, Total NBA Active Years, Total NBA Points by Top 50 College (rank by player's total minutes)
```python
fig = px.scatter_3d(df2, x = "points", y = "total_rebounds", z = "assists", opacity = 0.75,
                    color = "games", color_continuous_scale = "Blues",
                    title = "NBA Total Points Assists Rebound By College",
                    hover_name="college")

fig.update_traces(marker = dict(size = 3.5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```
![b58dc61762d481994ebea0db95050cc](https://user-images.githubusercontent.com/97984680/181853328-770746f2-83b1-46bb-a841-4dbb0181ff08.png)
```python
df8=draft[draft["games"]>82]
df9=df8.groupby(["college"]).count()
df10=df9[df9["id"]>=5]
df10["more_than_5_NBA_player"]=1
df10=df10.reset_index()
df11=df10[["college","more_than_5_NBA_player"]]
df12=df11.merge(df8,left_on="college",right_on="college",suffixes=('_left', '_right'))
df13=df12[df12["more_than_5_NBA_player"]==1]
df14=df13.groupby("college").mean().reset_index()
```

## Breakdown for NBA Player's Total minutes played, Total games played, Total active years (By College)
```python

for y in ["minutes_played_right", "games", "years_active"]:
    
    fig = px.bar(df4[df4["college_rank"]<50], x = "college_rank", y = y, title = f"Players From College Rank 1-50: {y}",
    color = "college_rank", color_continuous_scale = "Blues_r",
                hover_data=['player'],
                hover_name="college")

    fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 15))
    fig.show()
    print("")
 ```
 ![c8d989b2417626d34580b14063dbe37](https://user-images.githubusercontent.com/97984680/181853539-dd78e651-0fc9-436a-b0a7-28039d874873.png)
 
![b0f79b5c9c43ae3218ed8e8432595b6](https://user-images.githubusercontent.com/97984680/181853556-9b4673ae-14af-4bf7-b5e9-3c32cc125575.png)

![9551d0eda1473af626101a722e21f1f](https://user-images.githubusercontent.com/97984680/181853562-d5ddcf8e-5eb3-4580-abe7-eafeda385c34.png)

## Top 100 Scorers, Assister, Rebounders from college
```python
sorted_df = df4.sort_values(by = "points", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "player", y = "points", 
             title = "Top 100 NBA Scorers",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
iplot(fig)
```
![58145cc78da2e60a5398d9f044bdb35](https://user-images.githubusercontent.com/97984680/181853922-bbb63e64-3318-4a2e-bd69-0e86b63eb71c.png)

```python
sorted_df = df4.sort_values(by = "assists", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "player", y = "assists", 
             title = "Top 100 NBA Assister",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
iplot(fig)
```
![0e0b4ae9b0b092165ca08d30fa9e517](https://user-images.githubusercontent.com/97984680/181854031-69789a09-3f08-40c6-bf1c-5cc2ec45e186.png)

```python
sorted_df = df4.sort_values(by = "total_rebounds", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "player", y = "total_rebounds", 
             title = "Top 100 NBA Rebounders",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
iplot(fig)
```

![1af25edf0995748b11f191922e1171f](https://user-images.githubusercontent.com/97984680/181854043-6630587e-a6e2-4409-b6d4-ac4437fde7be.png)

# NBA Players' Average Stats Analysis (By College)

## Average Points, Assists, Rebound
```python
fig = px.scatter_3d(df14, x = "points_per_game", y = "average_total_rebounds", z = "average_assists", opacity = 0.75,
                    color = "points_per_game", color_continuous_scale = "Blues",
                    title = "NBA Average Points Assists Rebound By College (Only include college that have more than 5 NBA players)",
                    hover_name="college")

fig.update_traces(marker = dict(size = 3.5)) # scaling down the markers
fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 12))
fig.show()
```
![367eec23904a2bd499cc3eb4253c455](https://user-images.githubusercontent.com/97984680/181854155-ad913074-998e-4741-8190-6203662cfbb5.png)

## Players' Average 3 Pointer Percentage in NBA (By College)
```python
df5=draft[draft["games"]>82].groupby(["college"]).mean().sort_values(by=["minutes_played"],ascending=False)
df6=df5[["3_point_percentage","free_throw_percentage"]].reset_index()
df7=df3.merge(df6,left_on="college",right_on="college",suffixes=('_left', '_right'))
sorted_df = df7[df7["college_rank"]<30].sort_values(by = "3_point_percentage", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "college", y = "3_point_percentage", 
             title = "Top 30 Average 3-point-percentage(NBA) College",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```
![23bd20ae25f0c4b9f5c23accf7e9e2a](https://user-images.githubusercontent.com/97984680/181854278-9d94a4c7-4d98-4829-b906-d682a6e44572.png)

- By the result we got from the chart above, there is an interesting finding: In the Top 30 colleges, players from Texas and Villanova have the best 3-point shooting appearance and players from Georgetown and Stanford have the worst 3-point shooting appearance.

Let's find out what representative players we can get form these colleges

## Texas (3PT 32.95%)
```python
df8[df8["college"]=="Texas"].sort_values(by=["points"],ascending=False)
```
![b37d823637ee47ee107c4455f7b75ca](https://user-images.githubusercontent.com/97984680/181854490-5b91c86f-620f-429a-94c5-c316916db797.png)
![fec7f680a264d66ed3fb35ab36de1ac](https://user-images.githubusercontent.com/97984680/181854495-159fe90b-bb81-4984-bf10-c920c9811125.png)

## Villanova (3PT 32.94%)
```python
df8[df8["college"]=="Villanova"].sort_values(by=["points"],ascending=False)
```
![8406ab1a424ac2e7014ee3c61a56bbd](https://user-images.githubusercontent.com/97984680/181854713-5aff3c75-6f9e-4abe-89d4-22f3971360d8.png)
![2e17d153cbde872a034b46ae28128a8](https://user-images.githubusercontent.com/97984680/181854718-02a028bd-2f20-465c-877f-8fc3e5191d73.png)

## Georgetown (3PT 15.47%)
```python
df8[df8["college"]=="Georgetown"].sort_values(by=["points"],ascending=False)
```
![bad112e496d06a61cc027c058af06ee](https://user-images.githubusercontent.com/97984680/181854766-f6203830-dcfe-4702-8c76-557073edda5b.png)
![8efd4a4186e454b7115097ce2537e9d](https://user-images.githubusercontent.com/97984680/181854768-c4acf385-f3be-4a58-af72-6d223fd49ad9.png)

## Stanford (3PT 20.47%)
```python
df8[df8["college"]=="Stanford"].sort_values(by=["points"],ascending=False)
```
![52a9ab1b6bb246d6944de3db1c63a6d](https://user-images.githubusercontent.com/97984680/181854832-f64981aa-cf2c-4147-adab-e7fca87abe8e.png)
![7f691a3ccb1be6c0509e58e894c1cf6](https://user-images.githubusercontent.com/97984680/181854834-565615b6-ce75-4e4b-9a1e-d93d8527b67a.png)

## Players' Free-Throw Percentage in NBA Analysis (By College)
```python
sorted_df = df7[df7["college_rank"]<30].sort_values(by = "free_throw_percentage", ascending = False)

sorted_df = sorted_df.head(100)

fig = px.bar(sorted_df, x = "college", y = "free_throw_percentage", 
             title = "Top free-throw-percentage (NBA) by College",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```
![b1a29c334605a0377cd7a25a0d8fa98](https://user-images.githubusercontent.com/97984680/181854899-54b7c6bc-e313-4ddc-adc6-6e26dbed836d.png)

- In the top 30 college, USC has the best free throw percentage meanwhile Cincinnati has the worst free throw percentage

Let's find out what representative players we can get form these college

## USC (FT 76.33%)
```python
df8[df8["college"]=="USC"].sort_values(by=["points"],ascending=False)
```
![af0a7ff41d8f86166088593f39af6ea](https://user-images.githubusercontent.com/97984680/181854957-04cc02a7-d2b1-457c-8af0-eebda4aa3cf5.png)
![830d78fd196dd31b20c278e3fb7cc8a](https://user-images.githubusercontent.com/97984680/181854961-2a498a51-7c99-4039-bddd-683efb6cdae3.png)

## Cincinnati (FT 68.93%)
```python
df8[df8["college"]=="Cincinnati"].sort_values(by=["points"],ascending=False)
```
![554369fff1d379c3f8c87ab5c4a81da](https://user-images.githubusercontent.com/97984680/181855030-1d4ed869-76ab-4528-9078-eff33b3016a2.png)
![8261a6418433816c1ed31e5c6c9786a](https://user-images.githubusercontent.com/97984680/181855046-6da91508-d7ea-47b5-b29b-49305f7df3e3.png)

# NBA Survive Rate for players (By College)
- Survive Rate Defination: The player paly more than 82 * 3 = 246 games is defined as survived

- Here I selected the college which have 5 or more than 5 NBA players as the data sourece
```python
df15=df13[df13["games"]<246].groupby("college").count().reset_index()
df16=df15[["college","id"]]
df16.columns=["college","d_player"]

df17=df13[df13["games"]>=246].groupby("college").count().reset_index()
df18=df17[["college","id"]]
df18.columns=[["college","a_player"]]

f19=df17.merge(df16,left_on="college",right_on="college",suffixes=('_left', '_right'))
df19["Survive_Rate"]=df19["id"]/(df19["id"]+df19["d_player"])
df20=df19[["college","Survive_Rate"]]
df21=df20.sort_values(by=["Survive_Rate"],ascending=False)
df22=df21.merge(df3,left_on="college",right_on="college",suffixes=('_left', '_right'))

fig = px.bar(df22, x = "college", y = "Survive_Rate", 
             title = "NBA Survive Rate by College",
             color = "college_rank", color_continuous_scale = "Blues_r",
            hover_name="college")

fig.update_layout(template = "plotly_dark", font = dict(family = "PT Sans", size = 20))
fig.update_xaxes(showticklabels = False) # Hide x axis ticks
fig.show()
```
![6af2f2355f5bd2c3ddb2dcd31d17cb0](https://user-images.githubusercontent.com/97984680/181855152-65489b17-14b6-49aa-9e0e-4158dd50da1a.png)






 
