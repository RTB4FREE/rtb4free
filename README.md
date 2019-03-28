# RTB4FREE

RTB4FREE is an enterprise strength bidding platform for RTB based digital advertising.  RTB4FREE has the following functionality:

1. Campaign management
2. Data management (DMP) capability
3. Bidding using many different strategies
4. Analytics on historical media and pricing information

The [RTB4FREE](https://github.com/RTB4FREE) organization on Github contains all of the repos to run a fully scalable system.

The RTB4FREE/[rtb4free](https://github.com/RTB4FREE/rtb4free) repo is where all RTB4FREE user documentation, developer documentation, and API explorer documentation live.

## Viewing RTB4FREE Documentation

To view the documentation, navigate to:

https://rtb4free.readthedocs.io/en/latest/

## Building RTB4FREE Documentation

To build a local copy of the RTB4FREE documentation, follow these steps:

### Clone the rtb4free repo

Make a local directory and clone this repo:

```
mkdir rtb4free
cd rtb4free
git clone git@github.com:switzer/rtb4free.git
cd rtb4free
```
### Build the user documentation

Build the user documentation, which is built on [Sphinx](http://www.sphinx-doc.org/en/master/) and [ReadTheDocs](https://readthedocs.org/)

```
cd docs
make html
```

This will build HTML doc in the `rtb4free/docs/_build/html` directory.





