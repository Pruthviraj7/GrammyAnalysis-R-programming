# Assignment 2 - Grammy Awards Data Analysis

### Group: 3

---

### 📥 Data Import
```r
data <- read.csv("C:/T405/R programming/grammy_winners.csv", stringsAsFactors = FALSE)
head(data)
```
**This reads the Grammy Awards dataset and shows the first few rows to confirm it's loaded properly.**

---

### 🧠 Structure and Variables
```r
str(data)
names(data)
```
**This prints the structure and variable names of the dataset to understand its format.**

---

### 🔍 View Top Rows
```r
head(data, 15)
```
**Displays the first 15 rows to get an initial look at the records.**

---

### 🧮 User-defined Function: Awards by Year
```r
awards_by_year <- function(data) {
  year_counts <- table(data$year)
  year_counts <- sort(year_counts)
  return(year_counts)
}
awards_by_year(data)
```
**Creates a custom function that counts how many awards were given each year.**

---

### 🎯 Filter: Album Of The Year
```r
album_of_the_year <- subset(data, category == "Album Of The Year")
head(album_of_the_year)
```
**Filters the dataset to include only rows where the award category is 'Album Of The Year'.**

---

### 🔄 Reshape: Count Selected Artists by Year
```r
award_data <- data[, c("year", "artist")]
top_artists <- c("Beyoncé", "U2", "Stevie Wonder")
award_data <- subset(award_data, artist %in% top_artists)
award_data <- unique(award_data)
reshaped <- table(award_data$year, award_data$artist)
head(reshaped, 5)
```
**Creates a small reshaped table with only top 3 artists and how many times they won in each year.**

---

### 🧼 Missing Values Check & Removal
```r
nrow(data)
clean_data <- na.omit(data)
nrow(clean_data)
sum(is.na(data))
```
**Checks how many rows exist before and after removing missing values. Shows total NAs = 0 means data is clean.**

---

### 🧹 Duplicate Rows
```r
duplicated_rows <- data[duplicated(data), ]
head(duplicated_rows)
data_no_duplicates <- data[!duplicated(data), ]
head(data_no_duplicates)
nrow(data_no_duplicates)
```
**Identifies and removes duplicate entries from the dataset.**

---

### 🔃 Reorder Rows
```r
reordered_data <- data[order(-data$year, -xtfrm(data$artist)), ]
head(reordered_data)
```
**Reorders the data in descending order of year and artist name.**

---

### ✏️ Rename Columns
```r
colnames(data)[colnames(data) == "artist"] <- "artist_name"
colnames(data)[colnames(data) == "year"] <- "award_year"
names(data)
```
**Renames some columns to be more descriptive.**

---

### ➕ Add New Variable
```r
data$next_award_check_year <- data$award_year + 2
head(data[, c("award_year", "next_award_check_year")])
```
**Adds a new column to estimate a hypothetical next check year.**

---

### 📊 Create Training Set
```r
set.seed(123)
train_index <- sample(1:nrow(data), size = 0.7 * nrow(data))
training_set <- data[train_index, ]
head(training_set)
```
**Creates a random training dataset with 70% of the original rows.**

---

### 📈 Summary Statistics
```r
summary(data)
```
**Gives a summary of all numeric and character variables in the dataset.**

---

### 📉 Statistical Measures
```r
mean(data$award_year)
median(data$award_year)

get_mode <- function(x) {
  uniq_vals <- unique(x)
  uniq_vals[which.max(tabulate(match(x, uniq_vals)))]
}
get_mode(data$award_year)

range(data$award_year)
```
**Performs basic statistics on the award year column: mean, median, mode, and range.**

---

### 🔵 Scatter Plot: Awards Per Year
```r
award_counts <- table(data$award_year)
award_df <- as.data.frame(award_counts)
colnames(award_df) <- c("year", "award_count")
award_df$year <- as.numeric(as.character(award_df$year))

plot(award_df$year, award_df$award_count,
     main = "Awards Given Per Year",
     xlab = "Year",
     ylab = "Number of Awards",
     pch = 19,
     col = "blue")
```
**This scatter plot visualizes how many awards were given each year.**

---

### 🟧 Bar Plot: Awards Per Year
```r
barplot(award_counts,
        main = "Number of Awards Per Year",
        xlab = "Year",
        ylab = "Number of Awards",
        col = "orange",
        las = 2)
```
**Displays the number of awards each year in bar format for clearer comparison.**

---

### 🔗 Pearson Correlation: Year vs Categories
```r
category_counts <- aggregate(category ~ award_year, data = data, FUN = function(x) length(unique(x)))
cor(category_counts$award_year, category_counts$category, method = "pearson")
```
**Finds correlation between year and number of unique categories. Output shows strong positive correlation.**

> **Interpretation:** As the years increase, the number of award categories also increases — consistently.

---
