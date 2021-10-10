# Election_Analysis
**Overview of the Election Audit**
  
The purpose of this assignment was to analyze the largest voter counts by county and cadidates. A cadidate record was created that demonstrated the total votes in  the election. Each cadidate's total votes and percentage along with the winner of the election, the winning vote count and the winning percentage of votes. In      addition, a county record was also created, where each county's total vote count was presented as well as each county's percentage of total votes along with the county with the largest number of voters. Data was pulled in the script using a election_results.csv file. 

**Election Audit Results**

  According to the PyPoll analysis:
	
   - 369,711 votes were casted in the congressional election
   - Largest voter turnout was in Denver with a total of 306,055 votes, accounting for the 82.8% of the votes casted in the election
   - The next two counties were Jefferson with a total of 38,855 votes, accounting for 10.5% of the votes. Lastly, Arapahoe followed in last position with 24,801 votes accounting for only 6.7% of the total votes. 
   - From the above analysis it is apparent that Denver had largest county voter turnout. 
    
While, counties varied significantly in the election results, candidate results were just as scattered. Amongst the candidates, Diana DeGette recieved 73.8% of the total votes for a toal of 272,892. Charles Casper Stockham recieved 23.0% of the votes for a toal of 85,213 and lastly Raymon Anthony Doane recieved 3.1%  of the votes for a toal of 11,606 votes. 
    - Indubitably Diana DeGette won the elections with 272,892 making up 73.8% of the total votes. 
			<img width="1420" alt="Screen Shot 2021-10-10 at 1 53 49 AM" src="https://user-images.githubusercontent.com/90429568/136684279-ebf49f5b-5d05-4b65-9d50-46499e354ac4.png">

    
**Election Audit Summary**

The following script can be used for counting total votes and determining the winner based on the election results. Slight modifications in the script can also permit the user to understand why certain counties i.e. Arapahoe and Jefferson had lower voter turnout rate in comparison to Denver. Perhaps, polling stations were not easily accessible or election campaigns were not promoted enough to increase awareness that there is an election underway in the county. Of course, this would also require more data to be collected, i.e. number of polling stations available in each county. Additionally, if a user wanted to simply know the population numbers of each county to further analyze the election data, users can simply load the population CSV for each county to decipher if population sizes played a role. Overall, this script can be utilized in multiple ways as it has the capability of pulling in the data using CSV and perform calculations. 


    # -*- coding: UTF-8 -*-
		"""PyPoll Homework Challenge Solution."""

		# Add our dependencies.
		import csv
	import os

	# Add a variable to load a file from a path.
	file_to_load = os.path.join("..", "Election_Analysis", "Resources", "election_results.csv")
	# Add a variable to save the file to a path.
	file_to_save = os.path.join("Election_analysis", "Election_analysis.txt")

	# Initialize a total vote counter.
		total_votes = 0

	# Candidate Options and candidate votes.
	candidate_options = []
	candidate_votes = {}

	# 1: Create a county list and county votes dictionary.
	county_list = []
	county_votes = {}


	# Track the winning candidate, vote count and percentage
	winning_candidate = ""
	winning_count = 0
	winning_percentage = 0

	# 2: Track the largest county and county voter turnout.
	largest_county = ""
	largest_county_voter = 0


	# Read the csv and convert it into a list of dictionaries
	with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]



        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:


            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)


            # 4c: Begin tracking the county's vote count.
            county_votes[county_name]=0


        # 5: Add a vote to that county's vote count.
        county_votes[county_name] +=1



    # Save the results to our text file.
    with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in county_votes:

        # 6b: Retrieve the county vote count.
        countyvotes=county_votes.get(county_name)

        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(countyvotes) / float(total_votes) * 100
        county_results = (
            f"{county_name}: {county_vote_percentage:.1f}% ({countyvotes:,})\n")


         # 6d: Print the county results to the terminal.
        print(county_results, end="")

         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)

         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (countyvotes > largest_county_voter):
            largest_county_voter = countyvotes
            largest_county= county_name


    # 7: Print the county with the largest turnout to the terminal.
    winning_county_summary = (
    f"\n-----------------------\n"
    f"Largest County Turnout: {largest_county}\n"
    f"Winning County Vote: {largest_county_voter:,}\n"
    f"Winning County Name: {county_name:}\n"
    f"-------------------------\n")
    print(winning_county_summary)


    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)


    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
