# Volis Case Study: Educational Institution Analysis

## Project Summary
This project aims to analyze a dataset of educational institutions to understand various factors influencing their performance, identify key characteristics, and segment them into distinct groups. The primary goal is to provide insights into the landscape of colleges, including factors related to academic success and institutional types.

## Dataset
The analysis utilizes the `volis_dataset.csv` file. This dataset contains a variety of attributes for different colleges, including application statistics, student demographics, tuition costs, faculty information, and graduation rates.

## Analysis Overview

### 1. Data Loading
- Loaded `volis_dataset.csv` into a Pandas DataFrame. Displayed the head for initial inspection.

### 2. Dataset Overview
- Examined the dimension, data types, and statistical summary of the dataset.
- Separated numerical features into `X` and the `bl_private` column into `y`.
- Generated boxplots for all numerical variables, revealing the presence of outliers and varying scales.

### 3. Data Quality: Missing Values and Duplicates
- Identified missing values per column and imputed them using the median strategy, considering the presence of outliers.
- Confirmed no duplicate entries were present in the dataset.

### 4. EDA: Distributions and Outliers
- Developed a function (`outlier_summary`) to quantify upper and lower outliers, outlier percentages, and skewness for each variable.
- Applied Yeo-Johnson transformation to most numerical columns (excluding `pc_alumni_donors`, `qt_top_25_percent`, `pc_graduation_rate`, `pc_faculty_with_phd`, `pc_faculty_with_terminal_degree`) to address skewness and reduce outlier impact. This method was chosen for its effectiveness with both positive and negative values, and its ability to better handle both upper and lower outliers compared to logarithmic transformation.
- Visualized distributions of selected variables before and after transformation to demonstrate the effectiveness of the Yeo-Johnson method.

### 5. Directed Question: Correlation with Graduation Rate
- Analyzed the correlation of all features with `pc_graduation_rate` (graduation rate).
- Identified the top 3 features most positively correlated: `qt_top_25_percent`, `qt_top_10_percent`, and `pc_alumni_donors`. This suggests that institutions with academically stronger incoming students and higher alumni donations tend to have better graduation rates.
- Highlighted that while `vl_tuition_outstate` and `vl_room_board` also showed correlations, spurious correlation should be considered.

### 6. Student's Question: Alumni Donors in Public vs. Private Institutions
- Investigated if the percentage of alumni donors (`pc_alumni_donors`) differs between public and private institutions.
- Visualized the distribution using histograms and compared medians.
- **Key Finding**: Private institutions consistently show a significantly higher percentage of alumni donors compared to public institutions (median private: 23.00%, median public: 17.00%).

## Machine Learning Challenge: Clustering (Institution Segmentation)

### Data Preparation and Dimensionality Reduction
- Imputed any remaining missing values (though already handled in step 3, ensuring robustness).
- Standardized the dataset using `StandardScaler` to ensure all features contribute equally to the clustering process.
- Applied `PCA` (Principal Component Analysis), retaining components that explain 95% of the variance, to reduce dimensionality and facilitate visualization.

### Optimal Cluster Determination (K-Means)
- Used the elbow method (SSE) and silhouette score to determine the optimal number of clusters for K-Means.
- **Key Finding**: Both methods suggested **k=3** as the most appropriate number of clusters, with the highest silhouette score for K=3 at 0.21, indicating reasonably well-separated clusters.

### Cluster Profiles (K=3)
- Performed K-Means clustering with 3 clusters and visualized them using the first two principal components.
- Analyzed the centroids of each cluster to define their profiles, revealing distinct characteristics:
    - **Cluster 0 (Elite/High Investment Private)**: Characterized by a higher percentage of top students, higher alumni donations, more faculty with PhDs, greater expenditure per student, lower student/faculty ratio, and higher graduation rates. These institutions also have higher tuition and living costs. Approximately 95% of institutions in this cluster are private.
    - **Cluster 1 (Large Public)**: Characterized by a high number of applications received and students enrolled. These institutions generally show diminished characteristics compared to Cluster 0, such as lower alumni donations and less expenditure per student. Approximately 75% of institutions in this cluster are public.
    - **Cluster 2 (Small/Diverse Private)**: Characterized by a general decrease across most variables, suggesting smaller institutions. This cluster also shows a significant proportion of private institutions (around 72%), but with a less exclusive profile than Cluster 0.
- Noted that the `vl_books_cost` variable was irrelevant for clustering as it remained constant across all clusters.

### Comparison with Hierarchical Clustering
- Explored hierarchical clustering with the 'ward' method and visualized the dendrogram.
- Compared K-Means and Hierarchical clustering using a cross-tabulation of labels and the Adjusted Rand Score (ARS = 0.29).
- Compared silhouette scores: K-Means (0.21) outperformed Hierarchical clustering (0.13).
- **Conclusion**: K-Means with k=3 was retained as the preferred method due to its superior silhouette score, despite some overlap with hierarchical clustering.

## Conclusions and Reflections

1.  **Data Preprocessing**: Yeo-Johnson transformation proved most effective in managing outliers and skewness, crucial for clustering algorithms like K-Means.
2.  **Drivers of Academic Success**: The analysis strongly suggests that academic success (graduation rate) is positively influenced by the quality of incoming students (top 10/25 percent) and financial investment through alumni donations.
3.  **Institutional Diversity**: Unsupervised learning (K-Means) revealed three distinct institutional profiles, largely correlating with public/private status and investment levels, which would have been missed by purely supervised approaches.
4.  **Investment Importance**: There's a clear link between investment capacity (from donations, expenditure per student, lower student-faculty ratio) and better academic outcomes. Focusing on student support and investment is key to maximizing graduation rates.

## Limitations and Future Work

The current analysis is limited by the absence of data regarding the total budget and its allocation within institutions. Future work could involve:
-   Gathering data on **total budget** sources (tuition, donations, external funding, partnerships) and **expenditure targets** (professors, research, management).
-   Developing a **budgetary profile** for each institution to propose more targeted management methods aimed at maximizing graduation rates without compromising other essential college functions.


