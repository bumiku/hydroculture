# Hydroculture
Hydroculture is the growing of plants in a soilless medium, or an aquatic based environment. Plant nutrients are distributed via water.
## Techniques
In basic hydroculture or passive hydroponics, water and nutrients are distributed through capillary action. In hydroponics-like hydroculture, water and nutrients are distributed by some form of pumping mechanism.
## Substrates
The roots might be anchored in clay aggregate such as the trademarks LECA and Hydroton.
Advantages include ease of maintenance as watering and feeding involve just topping up the reservoir of growing solution. Certain types of hydroponic media are resistant to some types of soil-borne insects.

// This code requires the Nuget package Microsoft.AspNet.WebApi.Client to be installed.
// Instructions for doing this in Visual Studio:
// Tools -> Nuget Package Manager -> Package Manager Console
// Install-Package Microsoft.AspNet.WebApi.Client

using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Formatting;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace CallRequestResponseService
{
    class Program
    {
        static void Main(string[] args)
        {
            InvokeRequestResponseService().Wait();
        }

        static async Task InvokeRequestResponseService()
        {
            using (var client = new HttpClient())
            {
                var scoreRequest = new
                {
                    Inputs = new Dictionary<string, List<Dictionary<string, string>>> () {
                        {
                            "input1",
                            new List<Dictionary<string, string>>(){new Dictionary<string, string>(){
                                            {
                                                "Col1", "39"
                                            },
                                            {
                                                "Col2", "State-gov"
                                            },
                                            {
                                                "Col3", "77516"
                                            },
                                            {
                                                "Col4", "Bachelors"
                                            },
                                            {
                                                "Col5", "13"
                                            },
                                            {
                                                "Col6", "Never-married"
                                            },
                                            {
                                                "Col7", "Adm-clerical"
                                            },
                                            {
                                                "Col8", "Not-in-family"
                                            },
                                            {
                                                "Col9", "White"
                                            },
                                            {
                                                "Col10", "Male"
                                            },
                                            {
                                                "Col11", "2174"
                                            },
                                            {
                                                "Col12", "0"
                                            },
                                            {
                                                "Col13", "40"
                                            },
                                            {
                                                "Col14", "United-States"
                                            },
                                            {
                                                "Col15", "<=50K"
                                            },
                                }
                            }
                        },
                    },
                    GlobalParameters = new Dictionary<string, string>() {
                            {
                                "R Script", "# Map 1-based optional input ports to variables
dataset1 <- maml.mapInputPort(1) # class: data.frame
dataset2 <- maml.mapInputPort(2) # class: data.frame

# Contents of optional Zip port are in ./src/
# source("src/yourfile.R");
# load("src/yourData.rdata");

# Sample operation
colnames(dataset2) <- c(dataset1['column_name'])$column_name;
data.set = dataset2;

# You'll see this output in the R Device port.
# It'll have your stdout, stderr and PNG graphics device(s).
#plot(data.set);

# Select data.frame to be sent to the output Dataset port
maml.mapOutputPort("data.set");"
                            },
                            {
                                "Random Seed", "9"
                            },
                    }
                };

                const string apiKey = "abc123"; // Replace this with the API key for the web service
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
                client.BaseAddress = new Uri("https://ussouthcentral.services.azureml.net/workspaces/87a846b2cb4c41679c975361f275e84e/services/5af4abb723024d46ae47c1aa6cdbe494/execute?api-version=2.0&format=swagger");

                // WARNING: The 'await' statement below can result in a deadlock
                // if you are calling this code from the UI thread of an ASP.Net application.
                // One way to address this would be to call ConfigureAwait(false)
                // so that the execution does not attempt to resume on the original context.
                // For instance, replace code such as:
                //      result = await DoSomeTask()
                // with the following:
                //      result = await DoSomeTask().ConfigureAwait(false)

                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                if (response.IsSuccessStatusCode)
                {
                    string result = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Result: {0}", result);
                }
                else
                {
                    Console.WriteLine(string.Format("The request failed with status code: {0}", response.StatusCode));

                    // Print the headers - they include the requert ID and the timestamp,
                    // which are useful for debugging the failure
                    Console.WriteLine(response.Headers.ToString());

                    string responseContent = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(responseContent);
                }
            }
        }
    }
}

