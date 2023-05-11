# WhetherConsole

using System;
using System.Net.Http;
using System.Threading.Tasks; 
using Newtonsoft.Json;
namespace WeatherApp { class Program { static async Task Main(string[] args) { Console.WriteLine("Enter a city:"); string city = Console.ReadLine(); decimal latitude; decimal longitude; // Hardcoded latitude and longitude for Kolkata if (city.ToLower() == "kolkata") { latitude = 22.5411m; longitude = 88.3378m; } else { Console.WriteLine("City not found."); return; } using (HttpClient client = new HttpClient()) { string apiUrl = $"https://api.open-meteo.com/v1/forecast?latitude={latitude}&longitude={longitude}&current_weather=true"; HttpResponseMessage response = await client.GetAsync(apiUrl); if (response.IsSuccessStatusCode) { string json = await response.Content.ReadAsStringAsync(); WeatherData weatherData = JsonConvert.DeserializeObject<WeatherData>(json); Console.WriteLine($"Temperature: {weatherData.CurrentTemperature}°C"); Console.WriteLine($"Wind Speed: {weatherData.WindSpeed} km/h"); } else { Console.WriteLine("Failed to fetch weather data."); } } } } class WeatherData { [JsonProperty("temperature")] public decimal CurrentTemperature { get; set; } [JsonProperty("wind_speed")] public decimal WindSpeed { get; set; } } }
