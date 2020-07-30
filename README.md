# RYOT (run your own tests) jenkins workflow

This project wants to provide a jenkins-workflow to make scheduling and running
tests on a [RYOT-machine](https://github.com/fjmolinas/riot-ryot) easy.
It achieves this with a build.

## Setup

1. Setup a [RYOT-machine](https://github.com/fjmolinas/riot-ryot)
1. Setup a Jenkins Slave, for this your machine must make sure your
  [RYOT-machine](https://github.com/fjmolinas/riot-ryot)
   is reachable via ssh
1. Create the following pipeline jobs:

    - build-pipeline.jk (loads [build-pipeline.jenkinsfile](build-pipeline.jenkinsfile)
        - starts RIOT.git.jk
        - start build-boards.jk
        - collect and archive results
    - build-boards.jk (loads [build-boards.jenkinsfile](build-boards.jenkinsfile)
        - launches build-board.jk for every BOARD to be tested
    - build-board.jk (loads [build-board.jenkinsfile](build-board.jenkinsfile)
        - runs `compile_and_test_for_board.py` on one BOARD
    - RIOT.git.jk  (loads [RIOT.git.jenkinsfile](RIOT.git.jenkinsfile))
        - checkout's RIOT

## Optional

If multiple users might have access to the same machine consider adding a job
to run:.

- lock-boards.jk (loads [lock-boards.jenkinsfile](lock-boards.jenkinsfile))

This will simply create an ephemeral lock for a board or list of boards a user
might want to book for some time. If a board is locked any of the other build-%
jobs will simply skip running the test on that board.

This is also useful for letting colleagues know what boards you are using.
