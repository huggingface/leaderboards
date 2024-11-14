# C02 calculation

## Function for C02 calculation

To calculate `CO₂ Emissions for Evaluation (kg)` value, we use the following function. You can try to reproduce it yourself:

```python
def calculate_co2_emissions(total_evaluation_time_seconds: float | None) -> float:
    if total_evaluation_time_seconds is None or total_evaluation_time_seconds <= 0:
        return -1

    # Power consumption for 8 H100 SXM GPUs in kilowatts (kW)
    power_consumption_kW = 5.6
    
    # Carbon intensity in grams CO₂ per kWh in Virginia
    carbon_intensity_g_per_kWh = 269.8
    
    # Convert evaluation time to hours
    total_evaluation_time_hours = total_evaluation_time_seconds / 3600
    
    # Calculate energy consumption in kWh
    energy_consumption_kWh = power_consumption_kW * total_evaluation_time_hours
    
    # Calculate CO₂ emissions in grams
    co2_emissions_g = energy_consumption_kWh * carbon_intensity_g_per_kWh
    
    # Convert grams to kilograms
    return co2_emissions_g / 1000
```

## Explanation

The `calculate_co2_emissions()` function estimates CO₂ emissions in kilograms for a given evaluation time in seconds, assuming the workload is running on 8 NVIDIA H100 SXM GPUs in Northern Virginia.

Here’s how it works:

1. If `total_evaluation_time_seconds` is `None` or non-positive, the function returns `-1`, indicating invalid input.
   > Each result file have a `total_evaluation_time_seconds` field.

2. Assumes 8 NVIDIA H100 SXM GPUs with a combined power usage of 5.6 kilowatts (kW), based on each GPU’s maximum 0.7 kW consumption ([source](https://resources.nvidia.com/en-us-tensor-core/nvidia-tensor-core-gpu-datasheet)).

3. Uses an average of 269.8 grams of CO₂ per kilowatt-hour (g CO₂/kWh) for electricity in Virginia, based on U.S. Energy Information Administration data ([source](https://www.eia.gov/electricity/state/virginia/)).

4. Converts the evaluation time from seconds to hours, then calculates total energy usage in kWh.

5. Calculates emissions in grams by multiplying energy use (kWh) by the carbon intensity.

6. Finally, divides the total grams by 1,000 to convert to kilograms.
