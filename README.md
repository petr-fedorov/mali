# MArket LIquidity (MALI)

A stand-alone application for the transformation of the reconstructed order book events into a format suitable for a visualisation of market liquidity by [gnuplot](http://gnuplot.info/) or a similar application. An example of the visualisation (a few minutes of LINK-USD trading on Coinbase) is shown below:

![](mali.png)

Trades are shown as white or hardly coloured dots due to their small (relative to order book price level) volumes.

## Installation

The installation instructions are for Debian 10.

1. Install BOUML v7.10 or later as described [here](https://www.bouml.fr/download.html).

2. Install C++ Tool chain, Boost and Qmake:

       apt install clang-11
       apt install libboost-dev  libboost-system-dev libboost-thread-dev libboost-log-dev libboost-program-  options-dev
       apt install qt4-qmake

3. Launch BOUML and open `mali/mali.prj` project       
4. Generate C++ code for 'mali' and `.pro` file for `<executable>mali`. The code and .pro file will be generated in `/tmp/mali/cpp` folder.

5. Make `/tmp/mali/build` folder and build the application:

       mkdir /tmp/mali/build
       cd /tmp/mali/build
       qmake ../cpp/mali.pro
       make

## Usage

    ./mali [options] input-file-1 [input-file-2 ... input-file-N]

where `input-file-X` is a file containing reconstructed order book events (as produced by [OBERON](https://github.com/petr-fedorov/oberon)).

Options:

    -q [ --quote-increment ] arg (=0.01) specifies the minimum increment for the
                                         quote currency (i.e. USD in BTC-USD)
    -b [ --base-increment ] arg (=0.01)  specifies the minimum increment for the
                                         base currency (i.e. BTC in BTC-USD)

The program will output the following five files with self-explanatory names:
* `best_bids.csv`
* `best_asks.csv`
* `bids.csv`
* `asks.csv`
* `trades.csv`


Remove or rename `trades.csv` if you don't want to visualise trades.

Then launch gnuplot and enter the following commands:

    load ../cpp/mali.gp

    # adjust format and ranges as needed

    set format y "%10.5f"
    set yrange [12.58:12.67]
    set cbrange [-1000:1000]

    replot
