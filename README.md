<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
<body>

<h1>🍏 Nutritional Data Analysis with SQL</h1>
<p><strong>Author:</strong> Rahul Mahapatra</p>
<p><strong>Domain:</strong> Health Analytics | Diet Planning | SQL</p>

<h2>📌 Project Overview</h2>
<p>Analyzed a dataset of 870 food items using MySQL to uncover nutritional patterns, focusing on:</p>
<ul>
    <li>Macronutrients (protein, fats, carbs)</li>
    <li>Vitamins & Minerals (A, C, K, sodium, potassium)</li>
    <li>Dietary Insights (high-protein/low-fat foods, nutrient density)</li>
</ul>
<p><strong>Key Skills:</strong> Advanced SQL (subqueries, aggregations), Data Visualization, Nutritional Science Basics</p>

<h2>🗃️ Database Schema</h2>
<pre>
CREATE TABLE nutrition (
  food VARCHAR(255),
  caloric_value INT,
  fat FLOAT,
  saturated_fats FLOAT,
  protein FLOAT,
  dietary_fiber FLOAT,
  sodium FLOAT,
  potassium FLOAT,
  vitamin_a FLOAT,
  vitamin_c FLOAT,
  vitamin_k FLOAT,
  nutrition_density FLOAT,
  -- 20+ other columns
  PRIMARY KEY (food)
);
</pre>
<p>(Full schema available in SQL scripts)</p>

<h2>🔍 SQL Queries & Insights</h2>

<h3>Q1. Top 10 Highest-Calorie Foods</h3>
<pre>
SELECT food, MAX(caloric_value) AS calorie 
FROM nutrition 
GROUP BY food 
ORDER BY calorie DESC 
LIMIT 10;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Calories</th>
    </tr>
    <tr>
        <td>Banana Cream Pie</td>
        <td>3190</td>
    </tr>
    <tr>
        <td>Weetabix Weetabix</td>
        <td>2078</td>
    </tr>
</table>
<p>Insight: Desserts and processed foods dominate high-calorie list.</p>

<h3>Q2. High-Protein, Low-Fat Foods</h3>
<pre>
SELECT food, protein, fat 
FROM nutrition 
WHERE protein > 10 AND fat < 5 
ORDER BY protein DESC 
LIMIT 10;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Protein (g)</th>
        <th>Fat (g)</th>
    </tr>
    <tr>
        <td>Skipjack Tuna</td>
        <td>86.9</td>
        <td>4.0</td>
    </tr>
</table>
<p>Diet Tip: Ideal for weight loss/muscle gain.</p>

<h3>Q3. High-Sodium Foods with Saturated Fats >5g</h3>
<pre>
SELECT food, ROUND(AVG(sodium), 2) AS avg_sodium 
FROM nutrition 
WHERE saturated_fats > 5 
GROUP BY food 
ORDER BY avg_sodium DESC 
LIMIT 10;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Avg Sodium (g)</th>
    </tr>
    <tr>
        <td>Salt Mackerel</td>
        <td>6.1</td>
    </tr>
</table>
<p>Health Risk: Linked to hypertension.</p>

<h3>Q4. High-Fiber, Low-Sugar Foods</h3>
<pre>
SELECT food, dietary_fiber, sugars 
FROM nutrition 
WHERE dietary_fiber > 18 AND sugars < 4;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Fiber (g)</th>
        <th>Sugar (g)</th>
    </tr>
    <tr>
        <td>Wheat Bran</td>
        <td>24.8</td>
        <td>0.2</td>
    </tr>
</table>
<p>Benefit: Supports digestive health.</p>

<h3>Q5. Top 5 Vitamin C-Rich Foods</h3>
<pre>
SELECT food, MAX(vitamin_c) AS vitamin_c 
FROM nutrition 
GROUP BY food 
ORDER BY vitamin_c DESC 
LIMIT 5;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Vitamin C (mg)</th>
    </tr>
    <tr>
        <td>Clif Bar</td>
        <td>60</td>
    </tr>
</table>
<p>Note: Surprisingly, processed foods topped the list.</p>

<h3>Q6. Potassium-to-Sodium Ratio Extremes</h3>
<pre>
-- Highest Ratio 
SELECT food, ROUND(potassium/sodium, 2) AS ratio 
FROM nutrition 
ORDER BY ratio DESC 
LIMIT 1;

-- Lowest Ratio 
SELECT food, ROUND(potassium/sodium, 2) AS ratio 
FROM nutrition 
ORDER BY ratio ASC 
LIMIT 1;
</pre>
<table>
    <tr>
        <th>High-Ratio Food</th>
        <th>Ratio</th>
    </tr>
    <tr>
        <td>American Shad (Raw)</td>
        <td>706K</td>
    </tr>
    <tr>
        <th>Low-Ratio Food</th>
        <th>Ratio</th>
    </tr>
    <tr>
        <td>Unknown</td>
        <td>0.3</td>
    </tr>
</table>
<p>Health Insight: Balance electrolytes for heart health.</p>

<h3>Q7. Top Vitamin A + C Combo Foods</h3>
<pre>
SELECT food, vitamin_a, vitamin_c, (vitamin_a + vitamin_c) AS total 
FROM nutrition 
ORDER BY total DESC 
LIMIT 3;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Vit A (IU)</th>
        <th>Vit C (mg)</th>
        <th>Total</th>
    </tr>
    <tr>
        <td>Dessert Wine (Dry)</td>
        <td>362.7</td>
        <td>0</td>
        <td>362.7</td>
    </tr>
</table>
<p>Oddity: Alcohol-based items ranked high (data anomaly?).</p>

<h3>Q8. Top 10 Nutrition-Dense Foods</h3>
<pre>
SELECT food, nutrition_density 
FROM nutrition 
WHERE nutrition_density > (SELECT AVG(nutrition_density) FROM nutrition) 
ORDER BY nutrition_density DESC 
LIMIT 10;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Density Score</th>
    </tr>
    <tr>
        <td>Banana Cream Pie</td>
        <td>1533.5</td>
    </tr>
</table>
<p>Paradox: High-calorie foods scored high (review metric logic).</p>

<h3>Q9. Vitamin K-Dominant Foods</h3>
<pre>
SELECT food, vitamin_k, (vitamin_a + vitamin_b1 + ... + vitamin_k) AS total_vitamins 
FROM nutrition 
WHERE vitamin_k > 0.5 * total_vitamins;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Vit K (µg)</th>
        <th>Total Vitamins</th>
    </tr>
    <tr>
        <td>English Muffin</td>
        <td>166.4</td>
        <td>170.2</td>
    </tr>
</table>
<p>Benefit: Vital for blood clotting.</p>

<h3>Q10. High-Protein, Low-Sodium Foods</h3>
<pre>
SELECT food, protein, sodium 
FROM nutrition 
WHERE protein > 80 AND sodium < 80;
</pre>
<table>
    <tr>
        <th>Food</th>
        <th>Protein (g)</th>
        <th>Sodium (g)</th>
    </tr>
    <tr>
        <td>Skipjack Tuna</td>
        <td>86.9</td>
        <td>0.1</td>
    </tr>
</table>
<p>Diet Hack: Perfect for low-sodium, high-protein diets.</p>

<h2>📊 Key Findings</h2>
<ul>
    <li><strong>High-Calorie Culprits:</strong> Desserts like banana cream pie (3,190 cal).</li>
    <li><strong>Protein Powerhouses:</strong> Tuna and salmon offer >80g protein with minimal fat.</li>
    <li><strong>Sodium Alert:</strong> Processed meats and pies exceed daily limits.</li>
    <li><strong>Vitamin Gaps:</strong> Wine topping vitamin lists suggests data quality checks.</li>
</ul>

<div class="footer">
    <p>Created by Rahul Mahapatra | Data Analyst & SQL Enthusiast</p>
</div>

</body>
</html>
