# tiny-chess-01 entry

A 4 KB Python chess engine: a 0x88 board with alpha-beta search, quiescence
search, a transposition table, killer moves, MVV-LVA ordering, principal
variation search, null-move pruning and late move reductions. It evaluates
material plus piece-square tables, a bishop-pair bonus and a king table that
blends from middlegame to endgame as material comes off.

It searches to a fixed node budget (60,000 nodes a move, iterative deepening,
never a clock), so the same position always gets the same move. It talks to
the env alone over HTTP at `$MAKEFUN_ENV_URL` and plays every game of the
gauntlet.

The readable source is `engine.py`; `main.py` is that source packed with
`makefun compress`. No code is copied from the starter or from any other
engine; the piece-square tables follow the classic simplified evaluation
function, public chess knowledge.

## Local grading

Docker is not needed. From the kit directory, start the env for a practice
trial, run the bot against it, stop the env to export the state, then score:

    MAKEFUN_ENV_DATA_DIR=<dir with gauntlet.json> \
    MAKEFUN_STATE_DIR=<state dir> MAKEFUN_ENV_PORT=18080 \
    node program/env/server.mjs
    MAKEFUN_ENV_URL=http://127.0.0.1:18080 python3 main.py
    # SIGTERM the env, then:
    MAKEFUN_OUTPUT_DIR=<state dir>/export \
    MAKEFUN_REFERENCE_DIR=<kit>/practice/trial-N/reference \
    MAKEFUN_SCORES_DIR=<scores dir> node program/scorer/score.mjs

Local practice results at 60,000 nodes a move, with the final engine
(null-move pruning and late move reductions on): trial-0 Elo 1545
(4W/1D/1L, graded on the compressed main.py), trial-1 Elo 1654
(5W/0D/1L), trial-2 Elo 1545 (4W/1D/1L); mean 1581. The practice floor
is 1,154 and the house baseline (sunfish's 4 KB entry) rates about
1,365 on these trials. A 100,000-node trial-0 scored 1545 too, but its
slowest move took 9.1 s, too close to the 10 s limit, so the entry
ships at 60,000 nodes (slowest move 5.6 s, mean about 2.6 s).
