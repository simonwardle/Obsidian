The cli for Anaconda is conda

When you start a new terminal conda is configured to put you into the base environment.
![[Pasted image 20250614200954.png|450]]
Its is considered bad practice to work in the base env

## List environments.
```
conda env list
```

![[Pasted image 20250614201252.png|550]]

## Create a new environment

### Examples:

Create an environment containing the package 'sqlite':

```
conda create -n myenv sqlite
```

Create an environment (env2) as a clone of an existing environment (env1):

```
conda create -n env2 --clone path/to/file/env1
```
