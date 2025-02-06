# t_SNE

# Load necessary libraries
library(ggplot2)
install.packages("Rtsne")
library(Rtsne)

# Load the Iris dataset
data(iris)
# Remove duplicates to get unique data points
clean <- unique(iris)

# Increase dataset size by duplicating with noise
set.seed(123)  # For reproducibility
expanded_iris <- do.call(rbind, replicate(3, clean, simplify = FALSE))  # Triples the data
expanded_iris[, 1:4] <- expanded_iris[, 1:4] + matrix(rnorm(nrow(expanded_iris) * 4, 0, 0.2), ncol=4)

# Convert the dataset to matrix (only numerical features)
iris_matrix <- as.matrix(expanded_iris[, 1:4])

# Perform t-SNE
tsne_out <- Rtsne(iris_matrix, perplexity = 30, check_duplicates = FALSE)  # Adjust perplexity for better separation

# Convert matrix to dataframe and add species labels
tsne_plot <- data.frame(
  x = tsne_out$Y[, 1], 
  y = tsne_out$Y[, 2], 
  Species = expanded_iris$Species
)

# Enhanced visualization using ggplot2
ggplot(tsne_plot, aes(x = x, y = y, color = Species)) +
  geom_point(size = 3, alpha = 0.7) +  # Larger points with transparency
  theme_minimal() +  # Clean theme
  labs(title = "                                                    t-SNE ", x = "t-SNE 1", y = "t-SNE 2") +
  scale_color_manual(values = c("setosa" = "#E41A1C", "versicolor" = "#377EB8", "virginica" = "#4DAF4A")) +
  theme(legend.position = "none")
