> [!IMPORTANT]
> **Community maintenance fork.** The canonical research repository is [ICT-FinD-Lab/alphagen](https://github.com/ICT-FinD-Lab/alphagen). This personal fork is used for documentation, reproducibility triage, and carefully scoped compatibility work without modifying the authors' repository. See the [Community Maintenance Guide](COMMUNITY_MAINTENANCE.md) for audit status, known issues, and verification rules. No licensing status or official project ownership is changed by this fork.

# AlphaGen


<p align="center">
    <img src="images/logo.jpg" width=275 />
</p>


Automatic formulaic alpha generation with reinforcement learning.


This repository contains the code for our paper *Generating Synergistic Formulaic Alpha Collections via Reinforcement Learning* accepted by [KDD 2023](https://kdd.org/kdd2023/), Applied Data Science (ADS) track, publically available on [ACM DL](https://dl.acm.org/doi/10.1145/3580305.3599831). Some extensions upon this work are also included in this repo.


## Repository Structure


- `/alphagen` contains the basic data structures and the essential modules for starting an alpha mining pipeline;
- `/alphagen_qlib` contains the qlib-specific APIs for data preparation;
- `/alphagen_generic` contains data structures and utils designed for our baselines, which basically follow [gplearn](https://github.com/trevorstephens/gplearn) APIs, but with modifications for quant pipeline;
- `/alphagen_llm` contains LLM client abstractions and a set of prompts useful for LLM-based alpha generation, and also provides some LLM-based automatic iterative alpha-generation routines.
- `/gplearn` and `/dso` contains modified versions of our baselines;
- `/scripts` contains several scripts for running the experiments.


## Result Reproduction


Note that you can either use our builtin alpha calculation pipeline (see Choice 1), or implement an adapter to your own pipeline (see Choice 2).


### Choice 1: Stock data preparation


Builtin pipeline requires Qlib library and local-storaged stock data.


- READ THIS! We need some of the metadata (but not the actual stock price/volume data) given by Qlib, so follow the data preparing process in [Qlib](https://github.com/microsoft/qlib#data-preparation) first.
- The actual stock data we use are retrieved from [baostock](http://baostock.com/baostock/index.php/%E9%A6%96%E9%A1%B5), due to concerns on the timeliness and truthfulness of the data source used by Qlib.
- The data can be downloaded by running the script `data_collection/fetch_baostock_data.py`. The newly downloaded data is saved into `~/.qlib/qlib_data/cn_data_baostock_fwdadj` by default. This path can be customized to fit your specific needs, but make sure to use the correct path when loading the data (In `alphagen_qlib/stock_data.py`, function `StockData._init_qlib`, the path should be passed to qlib with `qlib.init(provider_uri=path)`).


### Choice 2: Adapt to external pipelines


Maybe you have better implements of alpha calculation, you can implement an adapter of `alphagen.data.calculator.AlphaCalculator`. The interface is defined as follows:


```python
class AlphaCalculator(metaclass=ABCMeta):
    @abstractmethod
    def calc_single_IC_ret(self, expr: Expression) -> float:
        'Calculate IC between a single alpha and a predefined target.'


    @abstractmethod
    def calc_single_rIC_ret(self, expr: Expression) -> float:
        'Calculate Rank IC between a single alpha and a predefined target.'


    @abstractmethod
    def calc_single_all_ret(self, expr: Expression) -> Tuple[float, float]:
        'Calculate both IC and Rank IC between a single alpha and a predefined target.'


    @abstractmethod
    def calc_mutual_IC(self, expr1: Expression, expr2: Expression) -> float:
        'Calculate IC between two alphas.'


    @abstractmethod
    def calc_pool_IC_ret(self, exprs: List[Expression], weights: List[float]) -> float:
