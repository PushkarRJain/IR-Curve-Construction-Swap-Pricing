# IR-Curve-Construction-Swap-Pricing
# Interest Rate Curve Construction & Swap Pricing Framework

[![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Quantitative Finance](https://img.shields.io/badge/domain-Quantitative%20Finance-orange.svg)]()
[![Trading](https://img.shields.io/badge/application-Rates%20Trading-red.svg)]()

A comprehensive Python framework for **interest rate curve construction**, **vanilla swap pricing**, and **advanced risk analytics** implementing post-crisis multi-curve derivatives pricing standards used by institutional trading desks.

## 🎯 **Project Overview**

This framework addresses the core need of every rates trading desk: **accurate, fast curve construction** with **comprehensive risk management**. Built for quantitative developers and risk managers in sell-side institutions requiring production-grade derivatives pricing infrastructure.

### **Key Features**
- ✅ **Multi-curve bootstrapping** (OIS for discounting, SOFR for forwarding)
- ✅ **Vanilla IRS pricing** with comprehensive Greeks calculation
- ✅ **Advanced risk analytics** (DV01, Key Rate Duration, Monte Carlo VaR)
- ✅ **Swaption pricing** with Black-Scholes model and volatility surface
- ✅ **Portfolio-level risk aggregation** and scenario analysis
- ✅ **Performance benchmarking** with sub-millisecond pricing
- ✅ **Professional visualizations** for risk management dashboards

## 🏗️ **Technical Architecture**

### **Core Components**

```
├── YieldCurve              # Base curve class with interpolation methods
├── MultiCurveFramework     # Post-crisis multi-curve implementation
├── CurveBootstrapper       # Market instrument calibration engine
├── VanillaSwap            # IRS pricing and risk metrics
├── SwaptionPricer         # European swaption valuation
├── AdvancedRiskAnalytics  # Key rate duration & Monte Carlo
├── MarketDataManager      # Data ingestion and benchmarking
└── Visualization Suite    # Professional charts and dashboards
```

### **Mathematical Foundation**
- **Bootstrapping**: Iterative root-finding to calibrate discount factors
- **Interpolation**: Cubic spline, log-linear, and linear methods
- **Risk Metrics**: Finite difference Greeks with parallel/bucket shifts
- **Monte Carlo**: Correlated interest rate scenario generation
- **Black-Scholes**: European swaption pricing with volatility surface

## 📊 **Performance Metrics**

| Metric | Value | Benchmark |
|--------|-------|-----------|
| **Curve Building** | 214ms avg | < 500ms target |
| **Swap Pricing** | < 1ms | Real-time capable |
| **Portfolio Analytics** | < 100ms | Production ready |
| **Monte Carlo (1K scenarios)** | 2.3s | Regulatory compliant |

## 🚀 **Quick Start**

### **Installation**

```bash
# Clone repository
git clone https://github.com/yourusername/ir-curve-swap-pricer.git
cd ir-curve-swap-pricer

# Install dependencies
pip install -r requirements.txt

# Run comprehensive analysis
python main.py
```

### **Dependencies**

```python
numpy>=1.21.0
pandas>=1.3.0
scipy>=1.7.0
matplotlib>=3.4.0
seaborn>=0.11.0
```

### **Basic Usage**

```python
from datetime import datetime
from ir_framework import *

# Initialize framework
reference_date = datetime(2025, 9, 14)
market_manager = MarketDataManager()
bootstrapper = CurveBootstrapper(reference_date)

# Load market data and build curves
market_data = market_manager.load_sample_market_data()
ois_curve = bootstrapper.bootstrap_ois_curve(market_data['USD_OIS'])
sofr_curve = bootstrapper.bootstrap_ois_curve(market_data['USD_SOFR'])

# Create and price vanilla swap
swap = VanillaSwap(
    notional=10_000_000,
    fixed_rate=0.0575,
    floating_tenor='3M',
    maturity='5Y',
    start_date=reference_date
)

# Calculate pricing and risk metrics
pv = swap.calculate_pv(ois_curve, sofr_curve)
dv01 = swap.calculate_dv01(ois_curve, sofr_curve)
convexity = swap.calculate_convexity(ois_curve, sofr_curve)

print(f"Present Value: ${pv:,.2f}")
print(f"DV01: ${dv01:,.2f}")
print(f"Convexity: {convexity:,.6f}")
```

## 📈 **Sample Results**

### **Curve Construction Performance**
```
Building yield curves...
Calculating swap metrics...
Running performance benchmarks...

CURVE BUILDING PERFORMANCE:
USD_OIS_avg_time_ms: 214.807
USD_OIS_std_time_ms: 31.220
USD_SOFR_avg_time_ms: 204.389
USD_SOFR_std_time_ms: 29.197
```

### **Swap Valuation & Risk Metrics**
```
SWAP DETAILS:
   Notional:        $10,000,000
   Fixed Rate:      5.75%
   Maturity:        5Y
   Payment Freq:    Quarterly

VALUATION & RISK METRICS:
   Present Value:   $66,895.82
   DV01:           $4,356.07
   PV01:           $4,356.07
   Convexity:      -205,600,266.158581
```

### **Advanced Portfolio Analytics**
```
PORTFOLIO SUMMARY:
   Number of Swaps:  3
   Total Notional:   $150,000,000
   Portfolio PV:     $-470,438.93
   Portfolio DV01:   $-30,234.03

KEY RATE DURATIONS (5Y Swap):
   2Y Duration:    -0.2843
   5Y Duration:     1.6011
   10Y Duration:   -0.0264

MONTE CARLO RISK METRICS (1,000 scenarios):
   Expected P&L:     $-254.71
   P&L Volatility:   $13,404.94
   95% VaR:         $-21,392.27
   99% VaR:         $-31,243.47
   Expected Shortfall: $-27,253.68
```

## 📊 **Visual Analytics Dashboard**

The framework generates comprehensive visualizations:

### **1. Interest Rate Curves**
- USD OIS vs SOFR term structure
- Forward rate evolution
- Basis spread analysis

### **2. Risk Analytics**
- Swap risk metrics comparison
- Key rate duration profiles
- Monte Carlo P&L distribution

### **3. Volatility Surface**
- 3D swaption volatility surface
- Heatmap visualization with strike/expiry grid

### **4. Portfolio Analytics**
- Position composition breakdown
- Risk factor decomposition
- Performance benchmarking

![Dashboard Sample](docs/images/dashboard_sample.png)

## 🏦 **Financial Engineering Features**

### **Multi-Curve Framework**
```python
# Post-crisis multi-curve setup
multi_curve = MultiCurveFramework(reference_date)
multi_curve.add_discount_curve(ois_curve)      # Fed Funds for discounting
multi_curve.add_forward_curve('3M', sofr_curve)  # SOFR for forwarding

# Handles OIS-SOFR basis spread
basis_spread = (sofr_curve.get_rate(5.0) - ois_curve.get_rate(5.0)) * 10000
print(f"5Y OIS-SOFR Basis: {basis_spread:.1f} bps")
```

### **Advanced Risk Analytics**
```python
# Key rate duration analysis
risk_analytics = AdvancedRiskAnalytics(multi_curve)
key_durations = risk_analytics.calculate_key_rate_duration(swap)

# Monte Carlo scenario analysis
mc_results = risk_analytics.monte_carlo_scenario_analysis(swap, num_scenarios=1000)
var_95 = mc_results['var_95']
expected_shortfall = mc_results['expected_shortfall_95']
```

### **Swaption Pricing**
```python
# Volatility surface construction
vol_surface = SwaptionVolatilitySurface()
vol_surface.build_sample_vol_surface()

# European swaption pricing
swaption_pricer = SwaptionPricer()
option_value = swaption_pricer.black_swaption_price(
    forward_swap_rate=0.0572,
    strike=0.0575,
    time_to_expiry=1.0,
    volatility=0.46,
    annuity=annuity_factor,
    option_type='call'
)
```

## 🔬 **Technical Implementation Details**

### **Curve Bootstrapping Algorithm**
1. **Market Instrument Ordering**: Sort by maturity (deposits → futures → swaps)
2. **Iterative Solving**: Use scipy root-finding to match market prices
3. **Interpolation Setup**: Cubic spline with boundary condition handling
4. **Validation**: Cross-check against market instrument fair values

### **Risk Metric Calculations**
- **DV01**: Finite difference with ±1bp parallel shifts
- **Key Rate Duration**: Localized bumps with triangular weighting
- **Convexity**: Second-order finite difference approximation
- **Monte Carlo**: Correlated shocks using exponential decay correlation

### **Performance Optimizations**
- **Vectorized Operations**: NumPy array computations
- **Efficient Interpolation**: Pre-computed spline coefficients
- **Memory Management**: In-place calculations where possible
- **Caching**: Store intermediate results for repeated calculations

## 📚 **Market Data & Calibration**

### **Supported Instruments**
- **Deposits**: O/N, 1W, 1M, 3M, 6M
- **Futures**: EDH5, EDM5, EDU5, EDZ5 (example)
- **Swaps**: 1Y, 2Y, 3Y, 5Y, 7Y, 10Y, 15Y, 20Y, 30Y

### **Sample Market Data**
```python
# USD OIS Curve Instruments
ois_instruments = [
    MarketInstrument('1M', 0.0525, 'deposit'),
    MarketInstrument('3M', 0.0535, 'deposit'),
    MarketInstrument('1Y', 0.0555, 'swap'),
    MarketInstrument('2Y', 0.0565, 'swap'),
    MarketInstrument('5Y', 0.0585, 'swap'),
    MarketInstrument('10Y', 0.0595, 'swap'),
    MarketInstrument('30Y', 0.0605, 'swap')
]
```

## 🧪 **Testing & Validation**

### **Unit Tests**
```bash
# Run comprehensive test suite
python -m pytest tests/ -v

# Test specific modules
python -m pytest tests/test_curve_construction.py
python -m pytest tests/test_swap_pricing.py
python -m pytest tests/test_risk_analytics.py
```

### **Validation Framework**
- **Cross-validation**: Bootstrap vs market instrument prices
- **Sensitivity Testing**: Greeks finite difference validation
- **Performance Benchmarking**: Latency and throughput testing
- **Numerical Accuracy**: Comparison against analytical solutions

## 📋 **Extension Roadmap**

### **Phase 1: Additional Products**
- [ ] Cross-currency swaps with FX forward curves
- [ ] Inflation swaps with real rate modeling
- [ ] Basis swaps (3M vs 6M LIBOR)
- [ ] Overnight index swaps (EONIA, SONIA)

### **Phase 2: Advanced Analytics**
- [ ] Bermudan swaption pricing (binomial/trinomial trees)
- [ ] CMS (Constant Maturity Swap) products
- [ ] Callable/Puttable bonds
- [ ] Credit default swaps with hazard rate curves

### **Phase 3: Production Features**
- [ ] Real-time market data integration (Bloomberg API)
- [ ] Database persistence (PostgreSQL/MongoDB)
- [ ] REST API for pricing services
- [ ] Docker containerization
- [ ] CI/CD pipeline with automated testing

### **Phase 4: Machine Learning Integration**
- [ ] Volatility surface forecasting
- [ ] Regime detection for risk models
- [ ] Automated curve fitting optimization
- [ ] Anomaly detection in market data

## 🏆 **Industry Applications**

### **Trading Desk Use Cases**
- **Market Making**: Real-time fair value calculation
- **Risk Management**: Daily P&L attribution and VaR reporting
- **Hedging**: Key rate duration matching for portfolio immunization
- **Structured Products**: Exotic derivatives pricing validation

### **Regulatory Compliance**
- **FRTB SA-CCR**: Standardized approach for counterparty credit risk
- **Basel III**: Regulatory capital calculation
- **CCAR/DFAST**: Stress testing scenarios
- **SR 11-7**: Model validation and governance

## 👥 **Contributing**

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) for details on:

- Code style and formatting standards
- Testing requirements and coverage
- Documentation standards
- Pull request process

### **Development Setup**
```bash
# Clone repository
git clone https://github.com/yourusername/ir-curve-swap-pricer.git
cd ir-curve-swap-pricer

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install development dependencies
pip install -r requirements-dev.txt

# Run pre-commit hooks
pre-commit install
```

## 📄 **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 **Acknowledgments**

- **Hull, John C.** - *Options, Futures, and Other Derivatives* (theoretical foundation)
- **Brigo, Damiano & Mercurio, Fabio** - *Interest Rate Models* (multi-curve framework)
- **QuantLib Community** - Reference implementation insights
- **Bloomberg Terminal** - Market data structure and conventions

---

## 🏷️ **Keywords for Recruiters**

`quantitative finance` `derivatives pricing` `interest rate models` `risk management` `monte carlo simulation` `yield curve construction` `swap pricing` `volatility modeling` `financial engineering` `python` `scipy` `numpy` `pandas` `matplotlib` `trading systems` `market data` `calibration` `bootstrapping` `greeks calculation` `value at risk` `key rate duration` `multi-curve framework` `ois` `sofr` `libor` `swaptions` `black-scholes` `portfolio analytics` `stress testing` `model validation` `performance optimization` `real-time pricing` `institutional trading` `sell-side` `buy-side` `rates trading` `fixed income` `systematic trading` `algorithmic trading`

---

⭐ **If you find this project useful, please consider giving it a star!** ⭐
