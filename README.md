# FIO based acceptance testing

Make a copy of one of the existing YAML files and adjust it to the expected
results.  The YAML files is then used to drive the pass/fail output of sbench


## Getting started

You will typically run sbench from a head node that has SSH access to a pool
of worker nodes.  The worker nodes must have fio installed and in the same
location as well as the storage mounted in the same location.

On the first run; sbench will create the data files, which should be greater
than the memory on the worker nodes in order to remove the impacts of caching
and thus truly exercise the backend storage.

It is possible to have all workers use the same data files or use unique
files.  This is controlled via the directory that you feed to sbench.

    Example:
    Same data:    /path/to/the/test/data
    Unique data:  /path/to/the/$(hostname -s)/data


## Usage

    usage: sbench [-h] [--noop] [-g GENDER] [-w TARGET] --test
                  {read,write,randread,randwrite,readwrite,randrw,data,iops,acceptance,burn}
                  --config YAML_FILE [--runtime SECONDS] [--colorize] [--debug]
                  [--verbose] [--pdsh_path PATH] [--fio_path PATH]
                  directory

    Storage benchmark & validation tool

    optional arguments:
      -h, --help            show this help message and exit
      --noop                Do not do anything (noop)

    required arguments:
      -g GENDER             pdsh gender to use (e.g. foo-all) ... either this or
                            "-w" is required
      -w TARGET             pdsh target to use (e.g. foo[01-04]) ... either this
                            or "-g" is required
      --test {read,write,randread,randwrite,readwrite,randrw,data,iops,acceptance,burn}
                            Type of test to run (use "data" to run both read and
                            write tests - use "acceptance" to run both data and
                            iops tests)
      --config YAML_FILE    Specify the YAML file with the expected results and
                            test options
      directory             Prefix filenames with this directory

    tuning arguments:
      --runtime SECONDS     Duration, in seconds, of the test

    output arguments:
      --colorize            Colorize the PASS/FAIL results
      --debug               Print debugging output
      --verbose             Print more verbose output

    optional arguments:
      --pdsh_path PATH      Path to the pdsh binary [default: /usr/bin/pdsh]
      --fio_path PATH       Path to the fio(binary [default: /usr/bin/fio]

