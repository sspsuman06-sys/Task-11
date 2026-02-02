# Step 1: Import Libraries
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# Step 2: Load Dataset
df = pd.read_csv('/content/marketing_AB.csv')

# Step 3: Split Control & Test Groups
control = df[df['group'] == 'control']['conversion']
test = df[df['group'] == 'test']['conversion']

# Step 4: Define Hypothesis
"""
H0: There is no difference between control and test group conversions
H1: There is a significant difference between control and test group conversions
"""

alpha = 0.05

# Step 5: Calculate Means
control_mean = control.mean()
test_mean = test.mean()

# Step 6: Perform T-Test
t_stat, p_value = stats.ttest_ind(control, test)

# Step 7: Confidence Interval
diff_mean = test_mean - control_mean
se = np.sqrt(control.var()/len(control) + test.var()/len(test))
ci = stats.norm.interval(0.95, diff_mean, se)

# Step 8: Visualization
plt.hist(control, alpha=0.6, label='Control Group')
plt.hist(test, alpha=0.6, label='Test Group')
plt.legend()
plt.title("A/B Testing Conversion Distribution")
plt.show()

# Step 9: Summary
summary = pd.DataFrame({
    'Group': ['Control', 'Test'],
    'Mean Conversion': [control_mean, test_mean]
})

summary
