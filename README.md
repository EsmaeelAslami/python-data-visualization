# Cell-type distribution
import os
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import matplotlib.font_manager as fm

print("Directory contents:", os.listdir("/content/"))

font_path = "/content/times.ttf"  # Make sure this path is correct.

try:
    times_font = fm.FontProperties(fname=font_path)
    plt.rcParams["font.family"] = times_font.get_name()
    print("✅ Font loaded:", times_font.get_name())
except Exception as e:
    print("❌ Error loading font:", e)
    times_font = None

### Sample data
data = {
    "Cell_type": [
        "BKL", "NV", "DF",
        "ML", "VASC", "BCC", "AKIEC"
    ],
    "Male": [190, 3400, 60, 610, 267, 580, 76],
    "Female": [137, 3200, 45, 475, 241, 515, 63],
    "Unknown": [0, 105, 10, 14, 6, 18, 3]
}

df = pd.DataFrame(data)

### Reshape data for Seaborn
df_melted = df.melt(id_vars="Cell_type", var_name="Sex", value_name="Count")

### Set figure size and resolution
plt.figure(figsize=(12, 6), dpi=800)  # Increase image resolution

### Create barplot
ax = sns.barplot(x="Cell_type", y="Count", hue="Sex", data=df_melted, palette=["blue", "orange", "green"], zorder=3)

### Set labels and title
plt.xlabel("Cell_type", fontsize=14, fontproperties=times_font, labelpad=15)  # Add space above label
plt.ylabel("Count", fontsize=14, fontproperties=times_font, labelpad=10)

### Set font for axis ticks (smaller size for better readability)
plt.xticks(fontproperties=times_font, fontsize=12, rotation=45)  # Smaller X-axis tick labels
plt.yticks(fontproperties=times_font, fontsize=12)  # Smaller Y-axis numbers

### Set grid only in background
ax.set_axisbelow(True)  # Grid lines appear behind the bars
plt.grid(axis='y', linestyle="--", alpha=0.5, color="gray", linewidth=0.7, zorder=0)

### Thicken axis lines for clarity + show top and right borders
plt.gca().spines["top"].set_visible(True)  # Show top border
plt.gca().spines["right"].set_visible(True)  # Show right border
plt.gca().spines["left"].set_linewidth(1.2)
plt.gca().spines["bottom"].set_linewidth(1.2)
plt.gca().spines["top"].set_linewidth(1.2)  # Thicken top border
plt.gca().spines["right"].set_linewidth(1.2)  # Thicken right border

plt.legend(fontsize=11, title_fontsize=10, prop=times_font, frameon=True, loc="upper right")

image_path = "cell_type_distribution_high_quality.png"
plt.savefig(image_path, dpi=800, bbox_inches="tight")

plt.show()
