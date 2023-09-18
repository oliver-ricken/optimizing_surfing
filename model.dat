# This code is written in AMPL, a Mathematical Programming Language for Operations Research tasks
  
# Sets
set Beaches;
set Times ordered;

# Ocean Parameters
param wave_height{Beaches, Times} >= 0;
param wave_period{Beaches, Times} >= 0;
param wind_direction{Beaches, Times} >= 0;
param wind_speed{Beaches, Times} >= 0;

# Road Parameters
param drive_time_to{Beaches, Times} >= 0;
param drive_time_from{Beaches, Times} >= 0;

# Bound Paramaters
#param wave_height_lower :=  
#param wave_period_lower := 
#param wind_speed_upper := 
#param drive_time_to_upper := 
#param drive_time_from_upper := 
param max_score_range := .3; 
param max_drive_time_range := 150;

# Score Parameters
param commute_score{i in Beaches, j in 6..18} = 1.7-((.5)*((.3)*(drive_time_to[i,j]/max{k in Times}drive_time_to[i,k])+.7)+((.5)*((.3)*(drive_time_from[i,j+2]/max{k in Times}drive_time_from[i,k])+.7))); 


param wave_score{i in Beaches, j in 6..18} = (1-(wave_period[i,j]/max{k in Times}wave_period[i,k]))*(wave_height[i,j]/max{k in Times}wave_height[i,k])
+ (1-(1-(wave_period[i,j]/max{k in Times}wave_period[i,k]))-wind_direction[i,j])*(wave_period[i,j]/max{k in Times}wave_period[i,k])
+(wind_direction[i,j])*(1-(wind_speed[i,j]/max{k in Times}wind_speed[i,k]));

# Decision Variables
var to_surf{Beaches, Times} binary;

# Objective Function
maximize stoke:
sum{i in Beaches, j in 6..18} (to_surf[i, j] * (wave_score[i, j] + commute_score[i, j]));

# Constraints

# Single beach and time (or none if conditions are bad)
subject to single_choice:
sum{i in Beaches, j in Times}to_surf[i,j] = 1;

# Avoid selecting extreme values for wave / commute score  
subject to avoid_extreme{i in Beaches, j in 6..18}:
abs(commute_score[i,j]-wave_score[i,j])*to_surf[i,j] <= max_score_range;

# Total driving limit
subject to driving_limit{i in Beaches, j in 6..18}:
(drive_time_to[i,j]+drive_time_from[i,j+2])*to_surf[i,j]<= max_drive_time_range;

# Can't surf at last time
subject to not_surfing_second_last_time{i in Beaches}:
to_surf[i, 19]=0;

subject to not_surfing_last_time{i in Beaches}:
to_surf[i, 20]=0;
