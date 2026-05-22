Hormuz Cryptoasset Repricing

This repository contains the replication materials, processed data, and database-ready Excel workbook for the study:

Hormuz Maritime Events and Cryptoasset Repricing

The project examines whether Hormuz-related maritime information events trigger heterogeneous repricing across cryptoassets. The empirical design focuses on two main Hormuz-related episodes and uses a seven-event specification as a robustness check. The analysis relates event-window cumulative abnormal returns to predetermined asset characteristics, especially exchange turnover and stablecoin settlement capacity exposure.

Repository contents
hormuz-crypto-repricing/
├── README.md
├── LICENSE
├── requirements.txt
├── code/
│   └── hormuz_repricing_with_excel_export_original_key.py
├── data/
│   └── processed/
│       └── hormuz_downloaded_data_for_database.xlsx
├── results/
│   └── tables/
└── docs/
    └── variable_definitions.md
Main script

The main replication script is:

code/hormuz_repricing_with_excel_export_original_key.py

The script performs the following tasks:

Downloads daily cryptoasset market data.
Downloads or reads stablecoin, on-chain, shipping, and macro-financial data.
Constructs pre-event exposure variables.
Computes event-window cumulative abnormal returns.
Estimates the main cross-sectional regressions.
Runs robustness checks, placebo timing tests, and macro interaction checks.
Exports all downloaded and constructed datasets into a multi-sheet Excel workbook for later database import.
API key note

The CoinGecko API key is already configured inside the script for local or private replication use. Therefore, the script can be run directly without manually setting a CoinGecko environment variable.

Important: because the API key is embedded in the source code, this repository should remain private unless the key is removed or replaced. Do not publish the script publicly with the API key still included.

The optional Artemis module still uses an environment variable if Artemis data are needed:

export ARTEMIS_API_KEY="your_artemis_key_here"

If no Artemis key is provided, the script will skip the optional Artemis P2P stablecoin transfer module and continue with the remaining data sources.

Data sources

The project uses the following data sources:

Data category	Source
Cryptoasset prices, market capitalizations, and exchange volumes	CoinGecko
Stablecoin prices, market capitalizations, and volumes	CoinGecko
Chain-level stablecoin supply	DefiLlama
On-chain transfer value, active addresses, and transaction counts	Coin Metrics Community API
Shipping indicators	IMF PortWatch local CSV files
Macro-financial controls	FRED, Cboe, DataHub/EIA, Yahoo Finance, Stooq, and CoinGecko
Hormuz event definitions	Public news event definitions used in the manuscript
Asset universe

The analysis uses thirty large, actively traded, non-stablecoin cryptoassets:

BTC, ETH, BNB, SOL, XRP, ADA, DOGE, TRX, TON, LTC,
XLM, BCH, ALGO, HBAR, AVAX, ATOM, ARB, OP, NEAR, SUI,
APT, DOT, ICP, ETC, FIL, LINK, UNI, AAVE, INJ, SEI

The settlement-capacity regressions are restricted to the chain-mapped subsample. Protocol or governance tokens without a unique settlement chain are kept in turnover regressions but are not assigned an artificial stablecoin settlement exposure.

Event design

The main design uses two Hormuz-related episodes:

February blockade episode
April Hormuz cluster, covering negotiation, toll-warning, escalation, and reopening signals

The robustness design uses seven dated Hormuz-related news events.

All CARs are computed from daily returns. The expected-return benchmark is a leave-one-out equal-weighted crypto market model, where each asset's market benchmark excludes that asset itself.

Key variables

The main explanatory variables are predetermined, pre-event asset characteristics.

Variable	Description
TURN	Pre-event exchange turnover intensity, measured by exchange volume relative to market capitalization
SETT	Pre-event stablecoin settlement capacity exposure, measured by chain-level stablecoin supply relative to asset market capitalization
HYB	Hybrid tradability and settlement exposure, defined as the average of standardized turnover and standardized settlement capacity
CAR	Event-window cumulative abnormal return

The main exposure window is the 90-day pre-event period. The expected-return model uses a 60-day estimation window with a six-day pre-event gap.

How to run

Install Python dependencies:

pip install -r requirements.txt

If requirements.txt is not yet available, install the core packages manually:

pip install pandas numpy requests statsmodels openpyxl

Run the main script:

python code/hormuz_repricing_with_excel_export_original_key.py

The script prints compact terminal output and writes the Excel workbook to the output folder.

Excel output

After successful execution, the multi-sheet Excel workbook is saved as:

hormuz_objective_episode_outputs/hormuz_downloaded_data_for_database.xlsx

This workbook is designed for later database upload. It contains downloaded raw-style panels, processed variables, event definitions, CAR panels, regression tables, diagnostics, and warning flags.

Typical sheets include:

metadata
data_dictionary
asset_master
chain_mapping
events_7
episodes_2
crypto_market_daily
stablecoin_market_daily
coinmetrics_daily
artemis_daily
defillama_stable_daily
portwatch_arrivals_daily
portwatch_volume_daily
portwatch_events_7
peg_diag_episodes
peg_diag_events_7
exposures_episodes
exposures_events_7
car_panel_episodes
car_panel_events_7
macro_daily
macro_download_diag
macro_events_7
reg_results_episodes
reg_results_events_7
mechanism_interactions
macro_interactions
placebo_pooled
placebo_distribution
warning_flags
Local PortWatch files

The script reads PortWatch shipping data from local CSV files if available. The expected candidate filenames include:

arrivals-of-ships.csv
arrivals-of-ships(1).csv
chart.csv
chart(1).csv

Place these files either in the working directory or update the candidate paths in the configuration section of the script.

If the PortWatch files are not found, the script will continue but the PortWatch-related tables may be empty.

Cache and outputs

The script uses a cache directory to avoid repeated downloads:

research_cache_objective_episode/

Main outputs are written to:

hormuz_objective_episode_outputs/

To force a clean re-download, delete the cache directory:

rm -rf research_cache_objective_episode

On Windows PowerShell:

Remove-Item -Recurse -Force .\research_cache_objective_episode
Reproducibility notes

The script is designed for daily event-window analysis only. It does not include an hourly-price module.

The main empirical design uses:

Sample window: 22 April 2025 to 19 April 2026
Effective data end: 19 April 2026
Main design: 2 episodes
Robustness design: 7 dated events
Exposure window: 90 pre-event days
Expected-return estimation window: 60 days
Pre-event gap: 6 days
Placebo simulations: 500 draws
Data redistribution note

The processed Excel workbook is provided for academic replication and database import. Some raw data are obtained from third-party providers and may be subject to provider-specific terms of use. Provider-restricted raw data and API credentials should not be redistributed in a public repository.

Citation

If using this repository, please cite the associated manuscript:

Hormuz Maritime Events and Cryptoasset Repricing.

A formal citation will be added after journal publication.

License

Code is released under the MIT License.

Processed data are provided for academic replication purposes, subject to the terms of the original data providers.
