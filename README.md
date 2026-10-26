# From optimal to lawful nurse rostering: data

This repository contains the data of the paper **"From optimal to lawful nurse rostering: a working-time-compliant
multi-objective model and a comparison of exact, hybrid and learning-based methods"** by Mominul Islam, Md. Mushiur
Rahman and Md. Limonur Rahman Lingkon (Department of Industrial and Production Engineering, Rajshahi University of
Engineering and Technology, Rajshahi 6204, Bangladesh). It holds the three benchmark scenarios, the 19 generated
instances, the definition of the subproblem solved on each generated instance, the starting rosters used in the
experiments and the published rosters and tables of Gülmez et al. (2026) in tidy form.

## 1. Contents

| Folder or file | Contents | Paper |
|---|---|---|
| `data/raw/WLB` | copy of the benchmark data of Gülmez et al. (2026) (see Section 7) | Section 5.1 |
| `data/instances/benchmark` | the benchmark scenarios S1, S2 and S3, one folder each | Section 5.1, Table 1 |
| `data/instances/generated` | the generated instances, one folder each: T1 and T2, A1 and A2, and L30_S1 to L200_S3 | Sections 5.1, 5.2, 5.4 and 5.6 |
| `data/instances/instance_summary.csv` | main characteristics of the 22 instances | Table 1 |
| `data/instances/subproblems.csv` | seed and bounds that define the subproblem of each generated instance (see Section 5) | Sections 5.1 and 5.6 |
| `data/starting_rosters` | the starting roster of each generated instance and of the benchmark scenario S1 (see Section 6) | Sections 5.2, 5.4 and 5.6 |
| `data/published_solutions` | the 36 rosters published by Gülmez et al. (2026) as files, with their objective values, fronts and values per staff member (see Section 7) | Sections 5.1, 5.3.1 and 5.3.2 |
| `data/published_tables` | values transcribed from the tables of Gülmez et al. (2026) (see Section 7) | Section 5.1 |

## 2. Names used in the files

| Name | Meaning |
|---|---|
| `S1`, `S2`, `S3` | benchmark scenarios; in generated instances, the pattern (shifts, requirements and requests) taken from that scenario |
| `T1`, `T2` | generated instances used to tune the hybrid methods (40 and 60 staff members, 28 days) |
| `A1`, `A2` | generated instances used in the study of the components (40 and 50 staff members, 28 days) |
| `L30_S1` to `L200_S3` | scalability instances: the number of staff members (30, 60, 100, 150 or 200) and the pattern |

The benchmark scenarios have 30 staff members and 21 days. All other instances have 28 days, except `L30_S1`,
`L30_S2` and `L30_S3`, which have 21 days.

## 3. Instance files

Every instance folder holds the same seven files. A slot is one shift on one day. Times are counted in hours from the
start of the planning horizon.

| File | Contents |
|---|---|
| `staff.csv` | `staff_id`, `staff_index`, `weekly_contract_hours_H` (weekly contract hours) and `requests_R` (number of requests) |
| `shifts.csv` | the shifts of a day: `shift_id`, `label`, `start_hour`, `end_hour`, `duration_hours` and `is_night` (1 for a night shift) |
| `requirements.csv` | one row per slot: `day`, `week`, `shift_id`, `label`, `start_time_b`, `finish_time_f`, `required_r` (staff members required) and `flexible_a` (1 if the slot allows a deviation of one staff member) |
| `requests.csv` | the requested slots, one row per request: `staff_id`, `day` and `shift_id` |
| `requests_matrix.csv` | the same requests as a table with one row per staff member and one column per slot (`d01_s1` is day 1, shift 1); 1 means requested |
| `parameters.json` | number of days and weeks, the days of each week, the numbers of the night shifts, the rule parameters (see below), the average share of the contracted hours `rho_bar`, the excess budget `V_excess_budget`, the total requirement and the total required and contracted hours |
| `derived_sets.json` | sets derived from the shifts: `windows_Omega` (slots inside each 24-hour window, by start time), `short_rest_pairs_P` (pairs of slots with less than the minimum rest between them), `chains_C` (minimal chains of back-to-back slots longer than the longest continuous duty) and `overlap_pairs_O` (pairs of overlapping slots; empty, as the shifts do not overlap) |

Rule parameters in `parameters.json` (the same for all instances):

| Name in the file | Meaning | Value |
|---|---|---|
| `G_min_rest_hours` | minimum rest between two separate duties | 11 hours |
| `L_max_continuous_hours` | longest continuous duty | 12 hours |
| `max_hours_in_24h` | most hours in any 24-hour period | 12 hours |
| `max_working_days_in_7` | most working days in seven consecutive days | 6 |
| `Gamma_unwanted_difference` | largest difference between staff members in the number of unwanted shifts | 3 |
| `Theta_night_difference` | largest difference between staff members in the number of night shifts | 2 |
| `beta_hours_band` | largest deviation of the share of contracted hours from the ward average | 0.10 |
| `V_excess_budget_share` | share of the total requirement allowed as total excess | 0.05 |
| `flexible_requirement_threshold` | smallest requirement of a slot that allows a deviation | 3 |
| `night_shift_start_before_hour` | a shift that starts before this hour is a night shift | 6 |

## 4. Generated instances

The generated instances keep the shifts of a pattern (S1, S2 or S3) and draw the weekly contracts, the requirements
(scaled with the number of staff members and the number of days) and the requests with a random seed. The seed of every
instance is given in `data/instances/subproblems.csv`. `instance_summary.csv` gives, for every instance, the number
of staff members, days, weeks, shifts, slots and assignment variables, the total requirement, the number of slots that
allow a deviation, the number of requests (in total and per staff member), the average share of the contracted hours
and the excess budget.

## 5. Subproblem of each generated instance

`data/instances/subproblems.csv` defines the subproblem that is solved on each generated instance: maximize the average
satisfaction F1 in flexible staffing (`staffing_tolerance` 1), with a total shortage F3 of at most `highest_shortage_F3`
(0) and a satisfaction rate of the least satisfied staff member F2 of at least `lowest_F2_percent`. The row `S1` gives
the subproblem of the benchmark scenario S1 in flexible staffing used in the study of the components.

## 6. Starting rosters

The starting rosters in `data/starting_rosters` are the rosters from which the search methods started. Each file has
one row per assignment, with the columns `staff_id`, `day`, `shift_id`, `label` and `requested_y` (1 if the slot was
requested by the staff member, 0 otherwise). Every starting roster meets all rules of the proposed model, has no
staffing shortage and has an F2 of at least the bound in `subproblems.csv`.

| Folder | Files | Used for |
|---|---|---|
| `tuning` | `T1.csv`, `T2.csv` | tuning of the hybrid methods (Section 5.2) |
| `components` | `A1.csv`, `A2.csv`, `S1_TOL1.csv` | study of the components (Section 5.6); `S1_TOL1.csv` belongs to the benchmark scenario S1 in flexible staffing |
| `scalability` | `L30_S1.csv` to `L200_S3.csv` | scalability study (Section 5.4); the first feasible roster found for the instance |

## 7. Data of Gülmez et al. (2026)

`data/raw/WLB` is a copy of the data files of the repository https://github.com/e-gulmez/WLB at commit
`21c14f288afcb30868cd39a80d92a87f8245bea0` (Gülmez, 2026). It holds the result files (`results/v12_results_*.json`),
the Excel reports of every published roster (`results/reports`) and the files of the revision of May 2026
(`results_revision_2026_05_24`). These files are included unchanged, so that the benchmark scenarios and the published
rosters can be traced to their source.

The folders `data/published_solutions` and `data/published_tables` contain the same published solutions and tables in
tidy form. In these files, `Z1_percent` is the average satisfaction (F1) and `Z2_deviation` the total staffing deviation
in the notation of Gülmez et al. (2026). The fairness levels `MEDIUM`, `HARD` and `EXTREME` have the thresholds
0.25, 0.15 and 0.05 on the range of the satisfaction rates, and `tolerance` is 0 for exact and 1 for flexible staffing.

| File | Contents |
|---|---|
| `data/published_solutions/rosters/` | the 36 published rosters (GA and NSGA-III, three scenarios, three fairness levels, two staffing modes), one file each with the columns `staff_id`, `day`, `shift_id`, `label` and `requested_y` |
| `data/published_solutions/rosters_index.csv` | one row per roster: algorithm, scenario, fairness level, staffing mode, file, number of assignments and the Excel report it was read from |
| `data/published_solutions/results.csv` | one row per roster: the objective values, range, variance, thresholds, run time and time stamp stored in the result file |
| `data/published_solutions/fronts.csv` | the points of the published NSGA-III fronts and the best GA value of each configuration |
| `data/published_solutions/individual.csv` | for every roster and staff member: satisfaction, worked hours, assigned shifts, requests and contract hours, from the result file and from the Excel report |
| `data/published_tables/paper_table9.csv` | values of Table 9 of Gülmez et al. (2026): objective values of GA and NSGA-III and the best value found |
| `data/published_tables/paper_tables18_20_summary.csv` | mean and standard deviation of the satisfaction rates printed in Tables 18 to 20 |
| `data/published_tables/paper_tables18_20_individual.csv` | satisfaction rate of every staff member printed in Tables 18 to 20 |

The files in `data/raw/WLB`, `data/published_solutions` and `data/published_tables` are the work of Gülmez et al.
(2026) and are not covered by the licence of Section 8. Please cite:

Gülmez, E., Aydin, M. E., Urganci, K. B., Gamoura, S. C., & Koruca, H. I. (2026). Healthcare staff scheduling with
work-life balance constraint using multi objective evolutionary algorithms. *Computers & Industrial Engineering, 220*,
Article 112287. https://doi.org/10.1016/j.cie.2026.112287

## 8. Licence

The instances, subproblem definitions and starting rosters produced in this study are released under the Creative
Commons Attribution 4.0 International Licence (CC BY 4.0), as stated in `LICENSE.md`. The files of Gülmez et al.
(Section 7) are excluded.
