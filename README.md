# dirac23.0_container_for_build

## Description

- This is a reproducible container for building DIRAC 23.0 on Debian 11 (bullseye)

## Usage

- Clone the repository:

```bash
git clone https://github.com/kohei-noda-qcrg/dirac23.0_container_for_build.git
```

- Build the container(bullseye)

```bash
cd dirac23.0_container_for_build/bullseye
docker compose up -d
```

- Enter the container:

```bash
docker exec -it dirac_build bash
```

- Build and test DIRAC:

```bash
./setup --mpi --fc=mpiifort --cc=mpiicc --cxx=mpiicpc && cmake --build build -j10 && ctest --test-dir ./build -j10
```
