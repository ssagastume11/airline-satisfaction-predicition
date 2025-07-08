# ✈️ Airline Satisfaction Prediction Analysis

**Author:** Sergio E. Sagastume 

**Repository:** [airline-satisfaction-predicition](https://www.kaggle.com/datasets/teejmahal20/airline-passenger-satisfaction)

---

## 📌 Introduction
This project explores airline passenger satisfaction survey data to understand what factors influence how satisfied travelers feel. By using data from Kaggle, I aim to identify patterns, build a model, and provide useful recommendations to improve airline services.

## 🌍 Scenario
Airlines rely heavily on feedback to improve customer experience and retain loyal passengers. With this dataset, I can look at survey ratings related to booking, boarding, comfort, and service, and explore how they connect to overall satisfaction.

---

## ❓ Ask
**Goal:** Understand what factors affect passenger satisfaction and create a model that predicts it.

---

## 📦 Prepare
**Dataset:** Kaggle – Airline Passenger Satisfaction  

**Source:** [Airline Passenger Satisfaction](https://www.kaggle.com/datasets/teejmahal20/airline-passenger-satisfaction)

```{r}
# Load libraries
library(tidyverse); library(janitor)

train <- read_csv("train.csv") %>%
  clean_names() %>%
  select(-id) %>%
  mutate(across(c(gender, customer_type, type_of_travel, class, satisfaction), as.factor),
         arrival_delay_in_minutes = replace_na(arrival_delay_in_minutes, 0),
         satisfaction_binary = ifelse(satisfaction == "satisfied", 1, 0))

```

---

## 🧹 Process

- Cleaned column Names
- Removed ID column
- Converted columns to factors
- Handling missing delays values
- Created binary outcome (satisfaction_binary)

```{r}
# Check for missing values
colSums(is.na(train))

# Final structure
glimpse(train)

```

## 📊 Analyze

![Age distributuion](https://raw.githubusercontent.com/ssagastume11/fifa-world-cup-analysis/refs/heads/main/team_age_exp.png)
```{r}
# Age distribution
ggplot(squads, aes(x = Age)) +
  geom_histogram(binwidth = 1, fill = "skyblue", color = "black") +
  labs(title = "Distribution of Player Ages in 2018 World Cup",
       x = "Player Age", y = "Count of Players",
       caption = "Source: 2018 FIFA World Cup Squad Data (CSV)") +
  theme_minimal()

```

---

## 📣 Share
This analysis was compiled using RMarkdown and includes visualizations and summaries to help sports enthusiasts and data students understand player distribution and team patterns during the 2018 tournament.

---

## ✅ Act
Results translated into useful results for readers:
```{r}
# Identify youngest and oldest teams
team_ages <- squads %>%
  group_by(Country) %>%
  summarise(avg_age = mean(Age, na.rm = TRUE)) %>%
  arrange(desc(avg_age))

print(team_ages)

```
This result helps compare the average ages of teams, which is useful for coaches, fans, and analysts who want to understand how age influences team strategy or tournament performance.

---

## 💡 Recommendations
1. **Explore Club Representation**: Analyze which clubs contributed the most players to the tournament.
2. **Extend to Match Stats**: Combine this with performance data to assess how age or club affiliation affects match outcomes.
3. **Visualize Geography**: Trace player nationality to see global patterns.
4. **Compare Across Tournaments**: Expand the dataset to include other FIFA World Cup years.
5. **Build Interactive Dashboard**: Use Shiny or Tableau for dynamic team and player exploration.
