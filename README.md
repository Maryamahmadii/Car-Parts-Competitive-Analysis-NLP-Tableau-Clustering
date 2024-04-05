# Comprehensive Analysis of Amazon Car Accessories Dataset

## Goals

The primary objective of this analysis is to explore and understand a dataset of car accessories scraped from the Amazon website, available on [Kaggle](https://www.kaggle.com/datasets/rayhan32/amazon-car-accessories-dataset). The analysis aims to provide a comprehensive understanding of the dataset through statistical analysis, clustering, and visual representation via a Tableau dashboard.

## Initial Data Processing

### Data Overview

The dataset comprises details of car accessories listed on Amazon, including product titles, categories, prices, stock information, and delivery details. Our goal is to categorize these products more accurately and analyze their distribution across different car accessory categories.

### NLP-Based Categorization

The first significant step in the analysis was to employ Natural Language Processing (NLP) techniques for categorizing products into subcategories. This process involved:

1. **Creating a Dictionary of Subcategories and Keywords:** A dictionary was developed where each subcategory is associated with a set of keywords extracted from product titles.

2. **Assigning Products to Subcategories:** Each product was assigned to a subcategory based on its title's match with the subcategory keywords. Products that could not be clearly categorized were assigned to an "Other" category.

## Data Preprocessing

After categorizing products, we dedicated efforts to preprocessing the dataset for enhanced data quality and readiness for in-depth analysis.

### Extracting Stock Numbers

We parsed stock information from strings to numeric format, enabling a more straightforward quantitative analysis of product availability.

### Delivery Date Cleaning

A series of steps were undertaken to ensure the delivery date information was accurate and consistent:

- **Weekday Name Removal:** Simplified date information by removing weekday names.
- **Date Range Separation:** For products with a delivery range, earliest and latest dates were identified. Single dates were treated as both the earliest and latest.
- **Scraping Date Assumption:** With the actual data scraping date unknown, the day before the earliest date in the dataset was assumed as the scraping date for calculations.
- **Format Standardization:** Implemented measures to ensure delivery date ranges were consistently formatted across the dataset.
- **Delivery Days Calculation:** Added columns to represent the number of days from order to delivery, improving the dataset's utility for further analysis.

### Column Exploration and Binary Indicators

Insights into promotional activities, warranty offerings, and purchase frequencies were gleaned through:

- **Unique Value Analysis:** Reviewed "Free Days" and "Past Month Purchases" columns for unique entries to detect any inconsistencies.
- **Binary Indicators Creation:** Introduced binary columns to flag products with promotions, warranties, and those frequently purchased, streamlining future analysis.

#### Imputing Missing Values
For the price column, which had 12 missing entries, values were imputed using the median price of each product's respective subcategory. This approach was chosen to maintain the integrity of the dataset while addressing the missing data in a manner that reflects the typical pricing structure within each subcategory.

#### Outlier Removal
To ensure the robustness of the analysis, outliers were identified and removed from the dataset. This step was critical in preventing skewed results and ensuring that the statistical analysis and clustering would be based on representative data.

#### Correlation Analysis
A heatmap of correlations between numerical variables was generated to explore the relationships within the data. Key findings include:
- **Price and Reviews:** A slight negative correlation (-0.054) suggests a weak and potentially insignificant relationship, where higher-priced items slightly tend to have fewer reviews.
- **Days to Earliest/Latest Delivery:** A very strong positive correlation (0.884) highlights the expected relationship between the days to earliest and latest delivery, which is expected, as for most of the products the values are the same.
- **Frequently Purchased and Promotions:** A moderate negative correlation (-0.486) indicates that items often purchased are less likely to have promotions, suggesting well-selling items don't require as much promotional effort.
- **Promotions and Warranty:** A negative correlation (-0.434) suggests that promotional items are less likely to offer extended warranties, possibly indicating a distinction in nature between promotional and regularly warrantied products.
- **Frequently Purchased and Number of Reviews:** The positive correlation (0.198) indicates that items frequently purchased tend to garner more reviews. This relationship is intuitive but weak, potentially due to the broad categorization of "frequently purchased" items based on a threshold of 50+ purchases in the last month without precise purchase counts.

These correlations reveal both expected and complex relationships within the dataset, emphasizing the nuanced nature of consumer behavior and the multifaceted factors influencing purchasing decisions.

### Logistic Regression Analysis

The subsequent step involved conducting a Logistic Regression analysis to explore the influence of various variables on the likelihood of an item being frequently purchased. The independent variables considered were Price, Number of Reviews, Stock Update, and Promotions, with the dependent variable being Frequently Purchased. The features were normalized and scaled, and the model was trained using a 70% training and 30% test split.

#### Classification Report and Confusion Matrix

The classification report indicated the following scores:
- Precision for class 0 (not frequently purchased): 0.80 and class 1 (frequently purchased): 0.72.
- Recall scores were 0.71 for class 0 and 0.81 for class 1.
- F1-scores were closely matched at 0.75 for class 0 and 0.76 for class 1.
- Overall accuracy of the model was 0.76.

#### Model Summary

- The Logit Regression Results highlighted a Pseudo R-squared of 0.3022, indicating the model explained approximately 30% of the variability in the dependent variable.
- A statistically significant LLR p-value (<0.000) suggested the model's predictors were meaningful in distinguishing between frequently and not frequently purchased items.
- Coefficients for the independent variables showed:
    - A negative relationship between Price and the likelihood of being frequently purchased.
    - A positive correlation for the Number of Reviews and Stock Update with being frequently purchased.
    - Promotions were negatively correlated with the likelihood of being frequently purchased.

### Adding Main Category to Predictions

In an effort to refine the model, Main Category variables were encoded using one-hot encoding and added as predictors. This adjustment aimed to assess if categorial distinctions within Main Categories could improve predictive performance.

#### Revised Classification Report and Confusion Matrix

After including Main Category variables:
- The model maintained an accuracy of 0.76.
- Precision, recall, and F1-scores remained consistent, underscoring the model's balanced performance in predicting both classes.

#### Revised Model Summary

- The inclusion of Main Category variables slightly increased the Pseudo R-squared to 0.3147, suggesting a minor improvement in the model's explanatory power.
- The Log-Likelihood improved, indicating a better fit to the data, yet the statistical significance of the Main Category variables was not uniformly established, as indicated by high p-values for most.

#### Conclusion

The logistic regression analysis revealed meaningful insights into the factors influencing whether an item is frequently purchased. While the addition of Main Category variables slightly enhanced the model's fit, their individual predictive significance was limited, suggesting that the initial set of predictors already captured the primary influences on purchasing frequency. This analysis underscores the nuanced impact of pricing, reviews, stock updates, and promotions on consumer purchasing behavior.

### Clustering Analysis

In preparing the dataset for clustering, I made the decision to focus on specific variables to optimize the analysis:

- **Days to Earliest Delivery:** Given its high correlation with Days to Latest Delivery, only this column was retained to avoid redundancy.
- **Exclusion of Subcategory Encoding:** Due to the substantial increase in dimensionality from one-hot encoding of subcategories, this feature was excluded from the clustering analysis.

#### K-means Clustering

K-means clustering was performed to segment the dataset into distinct groups based on their characteristics. Principal Component Analysis (PCA) was utilized for dimensionality reduction to facilitate visualization of the clusters.

#### Selection of K

The optimal number of clusters (K) was determined by evaluating the Silhouette Score for different numbers of clusters, ranging from 3 to 12. The analysis identified **K=5** as the best choice based on the highest silhouette score obtained.

#### Clustering Metrics

The clustering model's effectiveness was evaluated using several metrics:
- **Davies-Bouldin Index:** 0.7822506712709718, suggesting a reasonable separation between the clusters.
- **Calinski-Harabasz Index:** 4486.405253021266, indicating a strong cluster definition.
- **Silhouette Score:** 0.3665299220358104, reflecting moderate cluster cohesion and separation.

### Cluster Analysis Interpretation

The clustering revealed distinct segments within the dataset, each with unique characteristics and market positions:

#### Cluster 0: "Basic Needs, Low Engagement"
- Moderate price and the lowest number of reviews suggest these products might be essential but less popular or newer.
- Dominated by Automotive Tools, Body Parts, and Exterior Accessories, indicating a focus on basic maintenance and repairs.

#### Cluster 1: "Active Shoppers’ Choice"
- Similar to Cluster 0 in pricing but with higher engagement as shown by more reviews and stock updates.
- Slightly quicker delivery times and a balance between promotions and frequent purchases suggest a preference among active shoppers.

#### Cluster 2: "Long-Term Planners"
- Features the highest prices and high review numbers, but less frequent stock updates and longer delivery times, possibly indicating special-order items.
- This cluster likely represents specialized or high-demand products that require more planning and investment.

#### Cluster 3: "High Engagement, Everyday Users"
- Slightly above average price with significantly higher review numbers, indicating high consumer satisfaction and engagement.
- Quick delivery and the highest rate of frequently purchased items suggest loyal customers and efficient logistics.

#### Cluster 4: "Premium Selection"
- The highest price range with above-average reviews points to a premium segment.
- A good mix of tools, accessories, and body parts highlights a selection for enthusiasts or those seeking high-quality options.

This analysis successfully segmented the car accessories market into distinct clusters, each with unique consumer engagement levels and product offerings, providing valuable insights for targeted marketing and product development strategies.
