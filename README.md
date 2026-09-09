# Nestivo-Performance-Analysis
Repository for upGrad Data Analytics Bootcamp's Power BI Project

## Dashboard (Intro)

<img width="1278" height="718" alt="Dashboard Page 1 SS" src="https://github.com/user-attachments/assets/d04df8ed-7db8-411d-990c-880471436a91" />

Purpose: Project context, business need and navigation

## Executive Overview Dashboard

<img width="1277" height="718" alt="Dashboard Page 2 SS" src="https://github.com/user-attachments/assets/3a5fe5eb-40ce-44fd-a8f0-fd02fa21fcf3" />

Purpose: Scale, growth, market concentration and pricing

## Ratings & Reviews Dashboard (v1)

<img width="1278" height="716" alt="Dashboard Page 3 SS v1" src="https://github.com/user-attachments/assets/8a8c730c-0fa1-45b1-a4c3-e2260712eb35" />

Purpose: Detailed rating dimensions, seasonality and trust

## Ratings & Reviews Dashboard (v2)

<img width="1407" height="789" alt="Dashboard Page 3 SS v2" src="https://github.com/user-attachments/assets/49117567-0e48-4f78-b11f-9500a027fb03" />

Purpose: Overall ratings, seasonality and trust

## All Links

**Live Dashboard Link:** [Click Here](https://app.fabric.microsoft.com/view?r=eyJrIjoiMGNlMzVmMTMtMzQ5My00NjUxLTk3YjUtMjU0MDdhNzdiM2FhIiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9)

**Google Drive Link (contains all files):** [Click Here](https://drive.google.com/drive/folders/1S1itJZvgDRQhSqb1uhVbV36ItJTvOQiU?usp=sharing)

## Background

Nestivo is a fictional online vacation rental platform that connects guests with hosts offering short-term lodgings across major cities worldwide. Similar to Airbnb, the platform supports multiple property types, room categories, and hosts while serving millions of guest interactions through bookings and reviews. As Nestivo expands its marketplace across global destinations, leadership requires a centralised analytics solution to monitor platform growth, understand market concentration, evaluate pricing strategies, measure guest satisfaction, and assess host trust. Without an integrated reporting system, identifying high-performing cities, monitoring service quality, and making data-driven strategic decisions becomes increasingly challenging.

Current business challenges include: 

* Leadership lacks a unified dashboard that combines platform scale, growth, pricing, customer satisfaction, and trust metrics into a single executive view.  
* Business managers cannot quickly identify which cities contribute the most listings or determine whether marketplace supply is overly concentrated.  
* Pricing teams need better visibility into pricing differences across accommodation types to support revenue optimisation and market segmentation.  
* Customer Experience teams require a simple way to compare guest ratings across cities and identify the service dimensions affecting overall satisfaction.  
* Operations teams need insights into seasonal demand patterns to support localised planning instead of relying on a single global strategy.  
* Trust & Safety teams require visibility into host verification levels and profile completeness to strengthen customer confidence and platform security.  

To address these challenges, Nestivo has commissioned an interactive Power BI dashboard that provides executives and business teams with a comprehensive view of marketplace performance, customer experience, pricing behaviour, and host trust, enabling faster and more informed business decisions.

## Power BI Knowledge Applied

* Data Cleaning in Power Query

Here is the image:
<img width="1919" height="1031" alt="Data Cleaning in Power Query" src="https://github.com/user-attachments/assets/99cd834f-92ab-4d85-9965-40c328c9f221" />

* DAX
* Data Modelling

Here is the image:
<img width="1377" height="690" alt="Data Modelling" src="https://github.com/user-attachments/assets/ca3ebf27-2238-43c6-ab8e-e3ab183e337c" />

* KPI Creation
* Data visualisation
* Storytelling through Data

## Custom Columns Created

### review_month
```DAX
review_month = FORMAT(Reviews[date], "MMM")
```

### review_month_number
```DAX
review_month_number = MONTH(Reviews[date])
```

## Measures Created

### %_non_varified_has_profile_pic
```DAX
%_non_varified_has_profile_pic = DIVIDE([non_varified_has_profile_pic], [host_total], 0)
```

### %_non_varified_no_profile_pic
```DAX
%_non_varified_no_profile_pic = DIVIDE([non_varified_no_profile_pic], [host_total], 0)
```

### %_varified_has_profile_pic
```DAX
%_varified_has_profile_pic = DIVIDE([varified_has_profile_pic], [host_total], 0)
```

### %_varified_no_profile_pic
```DAX
%_varified_no_profile_pic = DIVIDE([varified_no_profile_pic], [host_total], 0)
```

### avg_accuracy
```DAX
avg_accuracy = AVERAGE(Listings[review_scores_accuracy])
```

### avg_cleanliness
```DAX
avg_cleanliness = AVERAGE(Listings[review_scores_cleanliness])
```

### avg_communications
```DAX
avg_communications = AVERAGE(Listings[review_scores_communication])
```

### avg_location_score
```DAX
avg_location_score = AVERAGE(Listings[review_scores_location])
```

### avg_price
```DAX
avg_price = AVERAGE(Listings[price])
```

### avg_rating
```DAX
avg_rating = AVERAGE(Listings[review_scores_rating])
```

### avg_value_score
```DAX
avg_value_score = AVERAGE(Listings[review_scores_value])
```

### city_rank
```DAX
city_rank = 
RANKX(
    ALL(Listings[city]),
    [total_listing],
    ,
    DESC
)
```

### cumulative_%
```DAX
cumulative_% = 
DIVIDE(
    [cumulative_listings],
    CALCULATE([total_listing], ALL(Listings[city]))
)
```

### cumulative_listings
```DAX
cumulative_listings = 
VAR current_rank =
    MAXX(
        VALUES(Listings[city]),
        [city_rank]
    )
RETURN
CALCULATE(
    [total_listing],
    FILTER(
        ALL(Listings[city]),
        [city_rank] <= current_rank
    )
)
```

### entire_place
```DAX
entire_place = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[room_type] = "Entire place"
)
```

### host_total
```DAX
host_total = DISTINCTCOUNT(Listings[host_id])
```

### hotel_room
```DAX
hotel_room = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[room_type] = "Hotel room"
)
```

### non_superhost_listings
```DAX
non_superhost_listings = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[host_is_superhost] = "f"
)
```

### non_varified_has_profile_pic
```DAX
non_varified_has_profile_pic = 
CALCULATE(
    [host_total],
    Listings[host_identity_verified] = "f",
    Listings[host_has_profile_pic] = "t"
)
```

### non_varified_no_profile_pic
```DAX
non_varified_no_profile_pic = 
CALCULATE(
    [host_total],
    Listings[host_identity_verified] = "f",
    Listings[host_has_profile_pic] = "f"
)
```

### private_room
```DAX
private_room = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[room_type] = "Private room"
)
```

### shared_room
```DAX
shared_room = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[room_type] = "Shared room"
)
```

### superhost_listings
```DAX
superhost_listings = 
CALCULATE(
    COUNT(Listings[listing_id]),
    Listings[host_is_superhost] = "t"
)
```

### total_listing
```DAX
total_listing = COUNT(Listings[listing_id])
```

### varified_has_profile_pic
```DAX
varified_has_profile_pic = 
CALCULATE(
    [host_total],
    Listings[host_identity_verified] = "t",
    Listings[host_has_profile_pic] = "t"
)
```

### varified_no_profile_pic
```DAX
varified_no_profile_pic = 
CALCULATE(
    [host_total],
    Listings[host_identity_verified] = "t",
    Listings[host_has_profile_pic] = "f"
)
```

### %_of_monthly_reviews
```DAX
%_of_monthly_reviews = 
DIVIDE(
    [total_reviews],
    CALCULATE(
        [total_reviews],
        ALLSELECTED(Listings[city])
        )
)
```

### distinct_reviewers
```DAX
distinct_reviewers = DISTINCTCOUNT(Reviews[reviewer_id])
```

### total_reviews
```DAX
total_reviews = SUM(Reviews[review_id])
```

## Conclusion

Nestivo has scale, a meaningful presence across global cities, strong guest ratings in several markets, a clear pricing ladder, and a generally healthy trust base. The main opportunity is not simply to grow listings further, but to improve consistency across cities, close weaker service gaps, and manage markets with a more localised strategy.
