# Shell Commands

---

## JupyterHub

1. Open shell environment and `cd` to `ELEE1119`, `$ cd ~/ELEE1119`
2. Make new directory, `$ mkdir shell_commands`

## 1. Dates

1. First run the `date` command in the terminal.

   ```sh
   $ date
   ```
   **Output:** will be something like this...

   ```sh
   Mon 27 Feb 13:58:41 GMT 2023
   ```
2. So how do we get different formats?

   Well if in doubt you can check the `man` pages or use the commands `--help` flag

   ```sh
   $ date --help
   ```

3. Looking at the output workout how to produce this format:

   ```sh
   > 20230227
   ```

   <details>
   <summary>Answer</summary>

   ```sh
   date +"%Y%m%d"
   ```

   </details>


4. Now modify this output to display:

   ```sh
   > Year: 2023, Month: 02, Day: 27
   ```

   <details>
   <summary>Answer</summary>

   ```sh
   date +"Year: %Y, Month: %m, Day: %d"
   ```

   </details>

5. The most common formatting characters:
   - `%D` – Display date as mm/dd/yy
   - `%Y` – Year (e.g., 2020)
   - `%m` – Month (01-12)
   - `%B` – Long month name (e.g., November)
   - `%b` – Short month name (e.g., Nov)
   - `%d` – Day of month (e.g., 01)
   - `%j` – Day of year (001-366)
   - `%u` – Day of week (1-7)
   - `%A` – Full weekday name (e.g., Friday)
   - `%a` – Short weekday name (e.g., Fri)
   - `%H` – Hour (00-23)
   - `%I` – Hour (01-12)
   - `%M` – Minute (00-59)
   - `%S` – Second (00-60)

6. Using the above information and what previous steps reproduce the ISO 8601 standard for datetime. 

   ```sh
   > 2023-02-27T12:08:45Z
   ```

   <details>
   <summary>Answer</summary>

   ```sh
   $ date +"%Y-%m-%dT%H:%M:%S:%NZ"
   ```

   - `%Y` – Year (e.g., 2020)
   - `%m` – Month (01-12)
   - `%d` – Day of month (e.g., 01)
   - `T` – self delimeter
   - `%H` – Hour (00-23)
   - `:` – self delimeter
   - `%M` – Minute (00-59)
   - `:` – self delimeter
   - `%S` – Second (00-60)
   - `Z` – self delimeter
   </details>

7. Amend the last command so that after seconds you get nano seconds.
   
   ```sh
   > 2023-02-27T12:14:03:281999558Z
   ```

   <details>
   <summary>Answer</summary>
   
   ```sh
   $ date +"%Y-%m-%dT%H:%M:%S:%NZ"
   ```

   - `%Y` – Year (e.g., 2020)
   - `%m` – Month (01-12)
   - `%d` – Day of month (e.g., 01)
   - `T` – self delimeter
   - `%H` – Hour (00-23)
   - `:` – self delimeter
   - `%M` – Minute (00-59)
   - `:` – self delimeter
   - `%S` – Second (00-60)
   - `:` – self delimeter
   - `%N` – nanoseconds (000000000..999999999)
   - `Z` – self delimeter
   </details>

----- 

## Task 2. Manipulating inputs and outputs

   1. working with the previous command lets strip out the `T`,`:`s and `Z` so that we are left with only numnbers.
   
   2. Using `awk` we can manipulate the output by piping `|` the output of `date` into awk
    - first run this command
      ```sh
      $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk "{print $1}" 
      > 2023-02-27T12:20:40:005619503Z   
      ``` 
      - `awk` with the argument `print $1` will print the first argument... ie the output of `date`
   3. Now lets remove the `T`
      ```sh
      $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F 'T' '{print $1}'
      > 2023-02-27
      ```
   4. What happens when you do $2 on the same command as above?
      <details>
      <summary>Answer</summary>

      ```sh
      > 12:50:48:341942600Z
      ``` 
      This is because the input has been split and now 
      - $1 = 2023-02-27
      - $2 = 12:20:40:005619503Z

      </details>

   5. Change the delimeter `-F 'T'` to `-F ':'` and repeat both steps as before:
      <details>
      <summary>Answer</details>
      
      ```sh
      $  date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F ':' '{print $1}'
      >  2023-02-27T12
      ```

      ```sh
      $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F ':' '{print $2}'
        55
      $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F ':' '{print $3}'
        32
      $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F ':' '{print $4}'
        473072761Z
      ```
      </details>

   6. You can put these together using the print {$2$3$4$}, try.
     ```sh
     date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F ':' '{print $2$3$4}'
     5719707652042Z
     ```
  7. Experiment with this and consider piping with awk to see if you can get:
 
     ```sh
     20230227130026355420256
     ```
     
     <details>
     <summary>Answer</summary>
     
     ```sh
     $ date +"%Y-%m-%dT%H:%M:%S:%NZ" | awk -F '[-T:Z]' '{print $1$2$3$4$5$6$7$8$9}'
     20230227130026355420256
     ``` 
     
     </details>


    