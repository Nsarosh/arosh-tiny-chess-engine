# tiny-chess-01 engine (AppuBot, @noman-siddik-arosh)

Compact deterministic chess engine for make.fun `tiny-chess-01`.

## Runtime
- `main.py` is the whole runtime engine (<=4096 bytes committed), generated from `engine.src.py` via:
  `node ../tiny-chess-01-kit-v18/tiny-chess-01-kit/cli/makefun.mjs compress engine.src.py`
  Run this as a separate command after every source edit.
- Python 3.13 stdlib only; talks only to `MAKEFUN_ENV_URL` (`POST /game`, `/move`, `/resign`). No model calls, no other network, no external/runtime files.

## Algorithm (fixed-node, deterministic)
- **State/replay:** parses FEN and replays game moves from `start_fen`/`moves` to track board, side, castling rights, en passant, halfmove clock, and repetition counts for draw-aware search.
- **Move generation:** full pseudo-legal generation with legal filtering (cannot leave king in check), including promotions, en passant (incl. discovered-check cases), castling with rights/empty/safe-transit and rook-presence checks; no king captures generated.
- **Search:** negamax alpha-beta with iterative deepening to a fixed node budget (hard cap checked in negamax/qsearch; incomplete iterations aborted and last completed best move retained). No wall-clock search.
- **Quiescence:** captures/promotions/en-passant captures at quiet nodes; in check searches all legal evasions and detects mate.
- **Draws/terminals:** repetition draw on third occurrence (`count >= 3`) and 50-move (`hm >= 100`) handled in search/qsearch; checkmate and stalemate recognized (including stalemate in quiescence).
- **Ordering:** TT move, captures by MVV-LVA, promotions/en-passant, killers, history heuristic.
- **Evaluation:** material + piece-square tables.
- **Root safety:** selected move must be in env `legal_moves`; otherwise falls back to first env legal move.

## Runtime loop
- `if __name__=="__main__"` driver polls `/game`, replays state, searches, and posts one move at a time; no clock in search, no model calls.

## Testing notes
- Local correctness tests cover start position, pawn attacks/checks, castling through/into check and rook presence, en-passant legality incl. discovered checks, promotions, pins, mate/stalemate, qsearch evasions in check, replay/search legality on all published practice opening positions, and fixed-node runtime proxy measurements. No official `grade-local`/Elo claimed here (no Docker daemon in this environment).

## Originality
Original implementation for this entry (not copied from Sunfish or other entrants).
