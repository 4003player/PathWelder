# PathWelder 🔥

**Join disconnected cut lines in PDF die files into continuous paths** — fewer cutter head lifts, faster cutting.

## The Problem

Box die lines from CAD software often arrive as individual line segments that share endpoints but aren't connected. The cutter treats each as a separate cut: lift → reposition → cut one segment → lift → reposition → repeat. On a complex die, this adds hundreds of unnecessary head movements.

## The Solution

PathWelder analyzes PDF content streams, finds line segments whose endpoints meet within a tolerance, and welds them into continuous paths. It also strips Illustrator's private roundtrip data (`/PieceInfo`) so the welded paths are respected when reopened.

**Example:** A shallow box die went from **24 subpaths → 9** (15 lifts eliminated). The entire box outline became one continuous cut path.

## Usage

1. Open [PathWelder](https://4003player.github.io/PathWelder/)
2. Drop a PDF with die lines
3. Review the Before/After preview (each subpath shown in a different color)
4. Adjust tolerance if needed (default 0.5pt)
5. Click **⚡ Weld Paths**
6. Click **💾 Export Welded PDF**

## How It Works

1. **Parses** PDF content streams via pako (zlib) decompression
2. **Extracts** vector path data (moveto, lineto, curveto operators) per stroke block
3. **Chains** subpaths whose endpoints meet within tolerance — supports forward, reverse, and curve joining
4. **Rebuilds** the content stream with welded paths using pdf-lib
5. **Strips** Illustrator private data (`/PieceInfo`, `%AI24_ZStandard_Data`) so the welded geometry takes priority

## Technical Notes

- Works with Illustrator, Acrobat, and other PDF-based die workflows
- Handles FlateDecode streams with both zlib-wrapped and raw deflate
- Supports Optional Content layers (OC/MC markers), CMYK color, spot colors
- Preserves all non-path content (clipping, graphics state, transforms)
- Tolerance is in PDF points (1pt = 1/72 inch)

## License

MIT
