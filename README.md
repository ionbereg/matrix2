// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © NeonMoney

//@version=5
indicator('󠀠', overlay=true)

TF = timeframe.period == '15S' ? '60' : timeframe.period == '1' ? '240' : timeframe.period == '3' ? 'D' : timeframe.period == '5' ? 'D' : timeframe.period == '15' ? 'W' : timeframe.period == '60' ? 'M' : timeframe.period == '240' ? '3M' : timeframe.period == 'D' ? '12M' : '12M'

timeflaga = timeframe.period == '1' ? true : false
timeflagb = timeframe.period == '3' ? true : false
timeflagc = timeframe.period == '15S' or timeframe.period == '15' ? true : false
timeflagd = timeframe.period == '60' ? true : false
timeflage = timeframe.period == '240' ? true : false
timeflagf = timeframe.period == 'D' ? true : false

out0 = ta.ema(close, 25 )
outa = timeflaga ? ta.ema(close, 125 ):na
outb = timeflaga ? ta.ema(close, 375 ):na
outc = timeflagb ? ta.ema(close, 125 ):na
outd = timeflagb ? ta.ema(close, 500 ):na
oute = timeflagc ? ta.ema(close, 100 ):na
outf = timeflagc ? ta.ema(close, 400 ):na
outg = timeflagd ? ta.ema(close, 100 ):na
outh = timeflagd ? ta.ema(close, 600 ):na
outi = timeflage ? ta.ema(close, 150 ):na
outj = timeflage ? ta.ema(close, 525 ):na
outk = timeflagf ? ta.ema(close, 175 ):na
outl = timeflagf ? ta.ema(close, 761 ):na
m=color.gray
n=75
plot(out0, color=m, transp=n)
plot(outa, color=m, transp=n)
plot(outb, color=m, transp=n)
plot(outc, color=m, transp=n)
plot(outd, color=m, transp=n)
plot(oute, color=m, transp=n)
plot(outf, color=m, transp=n)
plot(outg, color=m, transp=n)
plot(outh, color=m, transp=n)
plot(outi, color=m, transp=n)
plot(outj, color=m, transp=n)
plot(outk, color=m, transp=n)
plot(outl, color=m, transp=n)

//VWAP
timeflag = timeframe.period == '15S' or timeframe.period == '1' or timeframe.period == '3' or timeframe.period == '5' or timeframe.period == '15' or timeframe.period == '60' or timeframe.period == '240' or timeframe.period == 'D' ? true : false
src = hlc3
t = time(TF)
start = na(t[1]) or t > t[1]
sumSrc = src * volume
sumVol = volume
sumSrc := start ? sumSrc : sumSrc + sumSrc[1]
sumVol := start ? sumVol : sumVol + sumVol[1]
Value = timeflag ? sumSrc / sumVol : na
plot(Value, title="Vwap", style=plot.style_circles, linewidth=1, color=m, transp=n)

//Daily Pivot Plot
h = request.security(syminfo.tickerid, TF, high[1], barmerge.gaps_off, barmerge.lookahead_on)
l = request.security(syminfo.tickerid, TF, low[1], barmerge.gaps_off, barmerge.lookahead_on)
c = request.security(syminfo.tickerid, TF, close[1], barmerge.gaps_off, barmerge.lookahead_on)
p = (h + l + c) / 3.0
bc = (h + l) / 2.0
tc = p - bc + p
tcline = plot(timeflag ? tc : na, color=tc != tc[1] ? na : color.blue, linewidth=2, style=plot.style_line, display=display.none, transp=0)
bcline = plot(timeflag ? bc : na, color=bc != bc[1] ? na : color.blue, linewidth=2, style=plot.style_line, display=display.none, transp=0)
fill(tcline, bcline, color=tc != tc[1] and bc != bc[1] ? na : m, transp=92)
//Fib pivots
s1 = p - 0.236 * (h - l)
s2 = p - 0.382 * (h - l)
s3 = p - 0.618 * (h - l)
s4 = p - 0.786 * (h - l)
s5 = p - 1 * (h - l)
r1 = p + 0.236 * (h - l)
r2 = p + 0.382 * (h - l)
r3 = p + 0.618 * (h - l)
r4 = p + 0.786 * (h - l)
r5 = p + 1 * (h - l)

plot(timeflag ? p : na, linewidth=2, style=plot.style_line, color=p != p[1] ? na : m, transp=n)
plot(timeflag ? s1 : na, linewidth=1, style=plot.style_line, transp=n, color=s1 != s1[1] ? na : color.red)
plot(timeflag ? s2 : na, linewidth=1, style=plot.style_line, transp=n, color=s2 != s2[1] ? na : color.lime)
plot(timeflag ? s3 : na, linewidth=1, style=plot.style_line, transp=n, color=s3 != s3[1] ? na : color.aqua)
plot(timeflag ? s4 : na, linewidth=1, style=plot.style_line, transp=n, color=s4 != s4[1] ? na : color.orange)
plot(timeflag ? s5 : na, linewidth=1, style=plot.style_line, transp=n, color=s5 != s5[1] ? na : color.yellow)
plot(timeflag ? r1 : na, linewidth=1, style=plot.style_line, transp=n, color=r1 != r1[1] ? na : color.red)
plot(timeflag ? r2 : na, linewidth=1, style=plot.style_line, transp=n, color=r2 != r2[1] ? na : color.lime)
plot(timeflag ? r3 : na, linewidth=1, style=plot.style_line, transp=n, color=r3 != r3[1] ? na : color.aqua)
plot(timeflag ? r4 : na, linewidth=1, style=plot.style_line, transp=n, color=r4 != r4[1] ? na : color.orange)
plot(timeflag ? r5 : na, linewidth=1, style=plot.style_line, transp=n, color=r5 != r5[1] ? na : color.yellow)

//Table
var table t1 = table.new(position.bottom_center, 2, 7)
table.cell(t1, 1, 0, timeframe.period, text_color = color.new(m, n))
table.cell(t1, 0, 0, syminfo.ticker, text_color = color.new(m, n))
draw_line(_x1, _y1, _x2, _y2, _xloc, _extend, _color, _style, _width) =>
    dline = line.new(x1=_x1, y1=_y1, x2=_x2, y2=_y2, xloc=_xloc, extend=_extend, color=_color, style=_style, width=_width)
    line.delete(dline[1])


PreVal = 1
CurVal = 0

// Function outputs 1 when it's the first bar of the D/W/M/Y
is_newbar(tf) =>
    is_nb = 0
    t = time(tf)
    is_nb := ta.change(t) != 0 ? 1 : 0
    is_nb

[pd1_open, pd1_high, pd1_low, pd1_close] = request.security(syminfo.tickerid, TF, [open, high, low, close], lookahead=barmerge.lookahead_on)

var float pd_open = na
var float pd_high = na
var float pd_low = na
var float pd_close = na

if is_newbar(TF)
    pd_open := pd1_open[1]
    pd_high := pd1_high[1]
    pd_low := pd1_low[1]
    pd_close := pd1_close[1]

PP = (pd_high + pd_low + pd_close) / 3
S1 = PP - 0.236 * (pd_high - pd_low)
S2 = PP - 0.382 * (pd_high - pd_low)
S3 = PP - 0.618 * (pd_high - pd_low)
S4 = PP - 0.786 * (pd_high - pd_low)
S5 = PP - 1 * (pd_high - pd_low)
R1 = PP + 0.236 * (pd_high - pd_low)
R2 = PP + 0.382 * (pd_high - pd_low)
R3 = PP + 0.618 * (pd_high - pd_low)
R4 = PP + 0.786 * (pd_high - pd_low)
R5 = PP + 1 * (pd_high - pd_low)

pv_session = ta.barssince(is_newbar(TF) ? true : false)
pv_start_bar = bar_index - pv_session
pv_end_bar = pv_start_bar + 1


if true

    draw_line(pv_start_bar, PP, pv_end_bar, PP, xloc.bar_index, extend.right, color.new(color.gray, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, S1, pv_end_bar, S1, xloc.bar_index, extend.right, color.new(color.red, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, S2, pv_end_bar, S2, xloc.bar_index, extend.right, color.new(color.lime, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, S3, pv_end_bar, S3, xloc.bar_index, extend.right, color.new(color.aqua, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, S4, pv_end_bar, S4, xloc.bar_index, extend.right, color.new(color.orange, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, S5, pv_end_bar, S5, xloc.bar_index, extend.right, color.new(color.yellow, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, R1, pv_end_bar, R1, xloc.bar_index, extend.right, color.new(color.red, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, R2, pv_end_bar, R2, xloc.bar_index, extend.right, color.new(color.lime, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, R3, pv_end_bar, R3, xloc.bar_index, extend.right, color.new(color.aqua, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, R4, pv_end_bar, R4, xloc.bar_index, extend.right, color.new(color.orange, n), line.style_solid, 1)
    if true
        draw_line(pv_start_bar, R5, pv_end_bar, R5, xloc.bar_index, extend.right, color.new(color.yellow, n), line.style_solid, 1)

