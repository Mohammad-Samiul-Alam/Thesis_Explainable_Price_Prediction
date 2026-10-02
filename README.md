থিসিস নোট

Explainable Price Prediction of Consumer Electronics in Bangladeshi E‑Commerce — সহজ বাংলা সংস্করণ

1. [সমস্যা](#problem)
2. [ডেটাসেট](#dataset)
3. [পদ্ধতি](#method)
4. [মূল্যায়ন](#metrics)
5. [ব্যাখ্যাযোগ্যতা](#shap)
6. [গবেষণার ঘাটতি](#gap)
7. [প্রত্যাশিত ফলাফল](#outcome)
8. [সীমাবদ্ধতা](#limits)
9. [সুবিধা/সীমাবদ্ধতা](#tradeoffs)
10. [ভাইভা শিট](#viva)
11. [Reference](#refs)

আপনার রিসার্চ চ্যাট থেকে বাছাই করা জরুরি নোট

# থিসিসটা আসলে কী করে, এক পাতায়

একজন ক্রেতা বুঝতে পারে না কোনো ইলেকট্রনিক্স পণ্যের দাম ন্যায্য কি না। এই থিসিস পণ্যের তথ্য দেখে দাম predict করে, আর SHAP দিয়ে বুঝিয়ে দেয় কেন এই দাম এলো — তাই মডেলটা কখনো black box থাকে না।

Md. Samiul Alam · B.Sc. in CSE, Pundra University of Science & Technology

## সমস্যাটা আসলে কী

ধরুন Daraz-এ একটা Samsung smartphone আছে। একই ফোনের ক্ষেত্রে সাধারণত দেখা যায়:

- একজন seller চাইছে **৳25,000**, আরেকজন চাইছে **৳27,000**
- অন্য একটা platform-এ একই ফোনের দাম **৳29,000**
- কোথাও 10% discount, কোথাও 25% discount
- কোনো পণ্যে warranty আছে, কোনোটায় নেই

"এই দামটা কি আসলেই ঠিক আছে?" — এটা ক্রেতার প্রশ্ন।\
"কত দামে বেচলে আমি প্রতিযোগিতায় টিকে থাকব?" — এটা বিক্রেতার প্রশ্ন।

এই থিসিস এই দুই প্রশ্নেরই উত্তর খোঁজে ডেটা আর মেশিন লার্নিং দিয়ে, শুধু আন্দাজ করে না।

## ডেটাসেট

Daraz আর Pickaboo থেকে scrape করা প্রায় ৬,৫০০টি ইলেকট্রনিক্স পণ্যের তথ্য আছে। প্রতিটি পণ্যের নাম, ব্র্যান্ড, বিবরণ, মূল দাম, discount-কৃত দাম, discount %, warranty আর rating রয়েছে।

**Target variable: পণ্যের দাম।** Output একটা continuous সংখ্যা (৳25,000, ৳32,500…) বলে এটা *regression* সমস্যা, classification না।

## পদ্ধতি — পাঁচটি ধাপ

#### ডেটা পরিষ্কার করা

টেক্সট আকারের দাম ("৳25,000") সংখ্যায় রূপান্তর করা, duplicate আর অসম্পূর্ণ row বাদ দেওয়া, outlier সামলানো।

#### নতুন feature তৈরি করা

Product URL থেকে platform বের করা, নাম থেকে category বের করা, সাথে discount, warranty type আর description-এর দৈর্ঘ্য।

#### Log(price) ব্যবহার করা

দাম ৳1,000 থেকে ৳100,000+ পর্যন্ত ছড়ানো — খুব skewed। raw price-এর বদলে log(price) ব্যবহার করলে training স্থিতিশীল থাকে।

#### মডেল ট্রেইন করা

Linear Regression (সহজ baseline), Random Forest (অনেকগুলো tree), আর XGBoost (boosted, জটিল সম্পর্ক ধরতে পারে) — cross‑validation দিয়ে tune করা হবে।

#### SHAP দিয়ে ব্যাখ্যা

সবচেয়ে ভালো মডেলে SHAP প্রয়োগ করে দেখানো হবে কোন feature দাম বাড়াচ্ছে, কোনটা কমাচ্ছে — overall-ও, প্রতিটা পণ্যের জন্য আলাদাও।

## "ভালো" কি না সেটা কীভাবে মাপা হবে

MAE

গড়ে কত টাকা ভুল predict করছে। Predicted ৳28,000, আসল ৳30,000 হলে — ভুল ৳2,000।

RMSE

MAE-এর মতোই, তবে বড় ভুলকে আরও বেশি গুরুত্ব দিয়ে মাপে।

R²

দামের ওঠা-নামার কত অংশ মডেল ব্যাখ্যা করতে পারছে — 1-এর কাছাকাছি মানে ভালো।

## মডেলকে নিজের কাজ ব্যাখ্যা করানো

শুধু "৳35,000" বলে দেওয়াই যথেষ্ট না — এই থিসিসের মূল কথা হলো বিশ্বাসযোগ্যতা। SHAP সেই সংখ্যাটা ভেঙে দেখায় কোনটা দাম বাড়িয়েছে, কোনটা কমিয়েছে:

Brand = Samsung

Category = Smartphone

Warranty = ১ বছর

Discount প্রযোজ্য

Rating

এগুলো শুধু উদাহরণ — আসল bar-এর length আসবে ট্রেইন করা মডেল থেকে, এই পাতা থেকে না।

## গবেষণার ঘাটতি

বাংলাদেশি e‑commerce নিয়ে বেশিরভাগ গবেষণা customer review আর sentiment নিয়েই কাজ করে। ইলেকট্রনিক্স পণ্যের price modelling — আর মডেল কেন একটা নির্দিষ্ট দাম predict করল তা ব্যাখ্যা করা — এখনো তেমন explored না। এই থিসিসের contribution হলো চারটা জিনিস একসাথে আনা, যেগুলো সাধারণত একসাথে দেখা যায় না: **বাংলাদেশ + e‑commerce ইলেকট্রনিক্স + price prediction + explainable ML।**

## প্রত্যাশিত ফলাফল

| Price prediction model | পণ্যের তথ্য দেখে দাম অনুমান করতে পারবে। |
| --- | --- |
| দামের কারণ সম্পর্কে প্রমাণ | Brand, category, platform, warranty, discount-এর মধ্যে কোনটা সবচেয়ে বেশি প্রভাব ফেলে — SHAP থেকে বের হবে, আগে থেকে ধরে নেওয়া হবে না। |
| পুনঃব্যবহারযোগ্য cleaning pipeline | Scraped e‑commerce data পরিষ্কার করার ধাপগুলো অন্য গবেষকরাও পরে ব্যবহার করতে পারবেন। |

## যে সীমাবদ্ধতাগুলো শুরুতেই বলে রাখা ভালো

মাত্র দুইটা platform (Daraz, Pickaboo) থেকে প্রায় ৬,৫০০টি listing পুরো বাংলাদেশি বাজারকে প্রতিনিধিত্ব নাও করতে পারে। Data scraped বলে কিছু তথ্য missing বা ভুল থাকতে পারে। Demand, seller-এর সুনাম, stock, মৌসুমি প্রভাবের মতো গুরুত্বপূর্ণ বিষয় dataset-এ নেই, অথচ দাম সময়ের সাথে বদলায় আর মডেল একটা নির্দিষ্ট সময়ের data দিয়ে ট্রেইন করা হবে। গবেষণাটা শুধু ইলেকট্রনিক্সের মধ্যেই সীমাবদ্ধ — অন্য category-তে একই রকম কাজ করবে তার নিশ্চয়তা নেই। SHAP শুধু contribution দেখায়, causation প্রমাণ করে না — কোনো feature গুরুত্বপূর্ণ মানেই সেটা দামের আসল কারণ, এমন না।

Data leakage নিয়ে সতর্কতা: discounted price যদি listed price × discount %-এর সরাসরি হিসাব হয়, তাহলে এই সম্পর্কটা আবার feature হিসেবে মডেলে দেওয়া যাবে না — তাতে prediction কৃত্রিমভাবে সহজ হয়ে যায়।

## সুবিধা আর সীমাবদ্ধতা, পাশাপাশি

### সুবিধা

- পণ্যের একটা যৌক্তিক দাম predict করতে পারে
- শুধু সংখ্যা না, SHAP দিয়ে "কেন" সেটাও বলে
- একটা না, তিনটা মডেল তুলনা করে দেখে
- বিক্রেতা/platform-কে data‑driven pricing-এর দিকনির্দেশনা দেয়
- এই চারটা জিনিস একসাথে নিয়ে বাংলাদেশ‑কেন্দ্রিক প্রথম কাজ
- একটা পুনঃব্যবহারযোগ্য cleaning pipeline রেখে যায়

### সীমাবদ্ধতা

- মাত্র দুইটা platform — পুরো বাজার না
- Scraped data: missing/duplicate/ভুল row থাকতে পারে
- দাম সময়ের সাথে বদলায়; মডেল একটা মুহূর্তের data দেখায়
- Demand, stock, seller reputation ধরা হয়নি
- মাত্র তিনটা মডেল — অন্যগুলো ভিন্ন ফল দিতে পারে
- SHAP contribution দেখায়, causation প্রমাণ করে না

## "তোমার থিসিস কী?" জিজ্ঞেস করলে

কী

ইলেকট্রনিক্স পণ্যের দাম predict করা

কোথায়

বাংলাদেশের e‑commerce — Daraz ও Pickaboo

Data

প্রায় ৬,৫০০টি scraped listing

কীভাবে

পণ্যের তথ্যের ওপর ভিত্তি করে supervised regression

Models

Linear Regression, Random Forest, XGBoost

Evaluation

MAE, RMSE, R²

Explain

SHAP — overall আর প্রতিটা prediction-এর জন্য আলাদা

লক্ষ্য

স্বচ্ছ আর বিশ্বাসযোগ্য pricing বিশ্লেষণ

"আমার থিসিস brand, category, discount, warranty আর rating ব্যবহার করে Daraz আর Pickaboo-তে ইলেকট্রনিক্স পণ্যের দাম predict করে, Linear Regression, Random Forest আর XGBoost তুলনা করে — তারপর SHAP দিয়ে ব্যাখ্যা করে কোন feature কোন prediction-এ কতটা প্রভাব ফেলেছে। তাই এটা শুধু price prediction না, explainable price prediction।"

## সম্ভাব্য reference papers

এই শিরোনামগুলো আপনার আপলোড করা চ্যাটের একটা AI-সহায়তায় করা সার্চ থেকে এসেছে — আমি নিজে সার্চ করে verify করিনি। থিসিসে লেখার আগে Google Scholar বা publisher-এর site-এ গিয়ে প্রতিটা নিজে check করে নিন।

Machine Learning Study on Cross‑border E‑commerce Products Price Prediction (2024/25)

আপনার methodology-এর সবচেয়ে কাছের মিল — Lazada-এর electronics data, একই তিনটা মডেল (Linear Regression, Random Forest, XGBoost)।

Smart Pricing in Online Marketplaces: A ML and Analytics Framework (2025)

সরাসরি electronic‑device price prediction নিয়ে কাজ, একাধিক regression model তুলনা করা হয়েছে।

Product Pricing Solutions Using Hybrid Machine Learning Algorithm (2022/24)

Product spec-এর সাথে electronics pricing-এর সম্পর্ক, XGBoost-ভিত্তিক ensemble ব্যবহার করেছে।

Understanding Online Purchases with Explainable Machine Learning (2024)

আপনার explainability অংশের জন্য SHAP methodology-এর ভালো reference।

Explainable Machine Learning for E‑Commerce Purchase Intention (2026)

Price prediction না, কিন্তু একই model stack (RF/XGBoost + SHAP) — তুলনার জন্য ভালো।

### বাংলাদেশ‑কেন্দ্রিক research gap প্রতিষ্ঠার জন্য

Predicting On‑Time Delivery: Bangladesh E‑Commerce

Daraz shipment data + ML; local methodology-এর context বোঝার জন্য useful।

Sentiment Analysis of Bangladeshi E‑Commerce Site Reviews Using ML (2024)

দেখায় বিদ্যমান local গবেষণা মূলত review/sentiment নিয়ে — এটা আপনার research gap-কে সমর্থন করে।

A Multimodal Deep Learning Framework for Retail Price Estimation (2025)

বাংলাদেশ‑সংশ্লিষ্ট লেখক; price‑estimation methodology-এর সাথে প্রাসঙ্গিক।

Daraz/Pickaboo ইলেকট্রনিক্স price prediction-কে সরাসরি SHAP-এর সাথে combine করা — এমন কোনো verified published study পাওয়া যায়নি। এটাকে আপনার gap statement-এর একটা সম্ভাব্য framing হিসেবে ধরুন, নিশ্চিত সত্য হিসেবে না।

Proposal আর presentation তৈরির সময় দ্রুত দেখে নেওয়ার জন্য সংক্ষেপে সাজানো হয়েছে — এটা পুরো চ্যাট export বা মূল paper-গুলো পড়ার বিকল্প না।