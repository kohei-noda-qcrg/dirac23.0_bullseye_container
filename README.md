# dirac23.0_container_for_build

## Description

- This is a reproducible container for building DIRAC 23.0 on Debian 11 (bullseye) and AlmaLinux 8

## Usage

- Clone the repository:

```bash
git clone https://github.com/kohei-noda-qcrg/dirac23.0_container_for_build.git
```

- Build the container

  - bullseye

    ```bash
    cd dirac23.0_container_for_build/bullseye
    docker compose up -d
    ```

  - almalinux

    ```bash
    cd dirac23.0_container_for_build/almalinux
    docker compose up -d
    ```

- Enter the container:

  - bullseye

    ```bash
    docker exec -it dirac_build_bullseye bash
    ```

  - almalinux

    ```bash
    docker exec -it dirac_build_almalinux bash
    ```

- Build and test DIRAC:

```bash
export NPROCS=replace_with_number_of_cores_on_your_machine
./setup --mpi --fc=mpiifort --cc=mpiicc --cxx=mpiicpc && cmake --build build -j$NPROCS && ctest --test-dir ./build -j$NPROCS
```

## Test results (on my machine):

  - bullseye

    ```bash
    The following tests FAILED:
         21 - fscc_restart (Failed)
         43 - xyz_input (Failed)
         46 - cc_restart (Failed)
    Errors while running CTest
    Output from these tests are in: /workspace/dirac/build/Testing/Temporary/LastTest.log
    Use "--rerun-failed --output-on-failure" to re-run the failed cases verbosely
    ```

  - almalinux

    ```bash
    The following tests FAILED:
            21 - fscc_restart (Failed)
            31 - fde_response_mag (Failed)
            42 - fde_response_shield (Failed)
            43 - xyz_input (Failed)
            46 - cc_restart (Failed)
    Errors while running CTest
    Output from these tests are in: /workspace/dirac/build/Testing/Temporary/LastTest.log
    Use "--rerun-failed --output-on-failure" to re-run the failed cases verbosely.
    ```
