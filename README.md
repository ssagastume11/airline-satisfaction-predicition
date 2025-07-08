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

Built a logistic regression model to explore which features influence satisfaction the most.

```{r}
library(caret)

# Split data
set.seed(123)
train_index <- createDataPartition(train$satisfaction_binary, p = 0.8, list = FALSE)
train_set <- train[train_index, ]
test_set <- train[-train_index, ]

# Logistic regression
logit_model <- glm(satisfaction_binary ~ gender + customer_type + type_of_travel + class +
                     flight_distance + inflight_wifi_service + ease_of_online_booking +
                     online_boarding + seat_comfort + inflight_entertainment +
                     cleanliness + departure_delay_in_minutes + arrival_delay_in_minutes,
                   data = train_set, family = binomial)

summary(logit_model)

```

## 📣 Share
![Overall Passenger Satisfaction](https://raw.githubusercontent.com/ssagastume11/airline-satisfaction-predicition/refs/heads/main/Overall%20Passenger%20Satisfaction.png)
```{r}
# Plot: Overall satisfaction
ggplot(train, aes(x = satisfaction)) +
  geom_bar(fill = "#2c3e50") +
  labs(title = "Passenger Satisfaction Distribution",
       subtitle = "Source: Airline Passenger Satisfaction dataset on Kaggle",
       x = "Satisfaction", y = "Count") +
  theme_minimal()

```
![Average Service Ratings](https://raw.githubusercontent.com/ssagastume11/airline-satisfaction-predicition/refs/heads/main/Average%20Service%20Ratings.png)
```{r}
# Plot: Average service ratings
train %>%
  select(satisfaction, online_boarding, seat_comfort, inflight_wifi_service,
         inflight_entertainment, cleanliness) %>%
  pivot_longer(cols = -satisfaction, names_to = "service", values_to = "rating") %>%
  group_by(satisfaction, service) %>%
  summarise(avg_rating = mean(rating), .groups = "drop") %>%
  ggplot(aes(x = service, y = avg_rating, fill = satisfaction)) +
  geom_col(position = "dodge") +
  labs(title = "Average Service Ratings by Satisfaction Group",
       subtitle = "Source: Airline Passenger Satisfaction dataset on Kaggle",
       x = "Service", y = "Average Rating (1–5)") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

```

---

## ✅ Act
Most model accruacy on the test set was 86.03%, which is signifcantly better than guessing. Key predicitons of satisfaction include:
```{r}
# Top model coefficients
coeffs <- summary(logit_model)$coefficients
sorted_coeffs <- sort(coeffs[, "Estimate"], decreasing = TRUE)

head(sorted_coeffs, 5)
tail(sorted_coeffs, 5)

```
---

## 💡 Recommendations
1. **Improve Inflight Services**: Entertainment, seat comfort, and Wi-Fi strongly affect satisfaction.
2. **Work To Reduce delays**: Delays (especially on arrival) significantly reduce satisfaction.
3. **Support Loyal & Business Travelers**: Loyalty programs and premium travel options increase satisfaction.
4. **Enhance Digital Experience**: It's important to make online booking and boarding processes more seamless.
5. **Consider Expanding Buisness Class**: Business class passengers are more likely to be satisfied.
