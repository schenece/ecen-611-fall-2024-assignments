
# ECEN 611 Homework 4: Gap Function and Mutual Inductance for Salient Pole Rotor 

`Shuxuan Chen | 132006082 | Fall 2024`

<a name="beginToc"></a>

## Table of Contents
[Problem 3 ](#problem-3-)
 
&emsp;[Stator Counting & Winding Function ](#stator-counting-&-winding-function-)
 
&emsp;[Rotor Counting & Winding Function ](#rotor-counting-&-winding-function-)
 
&emsp;&emsp;[Original Rotor Counting Function φ ∈ \[θr θr+T\]](#original-rotor-counting-function-φ-∈-\[θr-θr+t\])
 
&emsp;&emsp;[Extended Rotor Counting Function φ ∈ \[θr\-2T θr+2T\]](#extended-rotor-counting-function-φ-∈-\[θr\-2t-θr+2t\])
 
&emsp;&emsp;[Extended Rotor Winding Function φ ∈ \[θr\-2T θr+2T\]](#extended-rotor-winding-function-φ-∈-\[θr\-2t-θr+2t\])
 
&emsp;[Stator Winding Magnetizing Inductance](#stator-winding-magnetizing-inductance)
 
&emsp;[Rotor Winding Magnetizing Inductance](#rotor-winding-magnetizing-inductance)
 
&emsp;[Stator\-Rotor Winding Mutual Inductance ](#stator\-rotor-winding-mutual-inductance-)
 
&emsp;[Torque Production](#torque-production)
 
&emsp;&emsp;[Reluctance Torque ](#reluctance-torque-)
 
&emsp;&emsp;[Alignment Torque ](#alignment-torque-)
 
&emsp;&emsp;[Total Torque](#total-torque)
 
&emsp;[Rotor Speed ωr](#rotor-speed-ωr)
 
&emsp;&emsp;[(a) When ω1 = ω2 = 0](#(a)-when-ω1-=-ω2-=-0)
 
&emsp;&emsp;[(b) When ω1 = ω2 ≠ 0](#(b)-when-ω1-=-ω2-≠-0)
 
&emsp;&emsp;[(c) When ω1 ≠ 0, ω2 = 0](#(c)-when-ω1-≠-0,-ω2-=-0)
 
<a name="endToc"></a>

# Problem 3 

![](./ECEN611_HW4_Problem_3_media//)

```matlab
syms I positive real 
syms N_s 

syms phi 
syms theta_r 
assume( 0 <= theta_r <= 2*pi )

NS = 1;

T = 2*pi;

digits(4)
```

## Stator Counting & Winding Function 
```matlab
statorCountingFunction(phi) = piecewise( ...
    0 <= phi < pi/2, 0, ...
    pi/2 <= phi < 3/2*pi, -N_s, ...
    3/2*pi <= phi < T, 0 ...
);

disp("Original Stator Counting Function φ ∈ [0 T] :")
```

```matlabTextOutput
Original Stator Counting Function φ ∈ [0 T] :
```

```matlab
disp(statorCountingFunction(phi))
```
 $\displaystyle \left\lbrace \begin{array}{cl} 0 & \;\textrm{if}\;\;\phi \in \left\lbrack 0,\frac{\pi }{2}\right)\newline -N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{\pi }{2},\frac{3\,\pi }{2}\right)\newline 0 & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right) \end{array}\right.$
 

```matlab

statorCountingFunction_ext(phi) = piecewise( ...
    -2*T <= phi < -T, statorCountingFunction(phi+2*T), ...
    -T <= phi < 0, statorCountingFunction(phi+T), ...
    0 <= phi < T, statorCountingFunction(phi), ...
    T <= phi <= 2*T, statorCountingFunction(phi-T) ...
);

disp("Extended Stator Counting Function φ ∈ [-2T 2T] :")
```

```matlabTextOutput
Extended Stator Counting Function φ ∈ [-2T 2T] :
```

```matlab
disp(statorCountingFunction_ext(phi))
```
 $\displaystyle \left\lbrace \begin{array}{cl} 0 & \;\textrm{if}\;\;\phi \in \left\lbrack -4\,\pi ,-\frac{7\,\pi }{2}\right)\newline -N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{7\,\pi }{2},-\frac{5\,\pi }{2}\right)\newline 0 & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{5\,\pi }{2},-\frac{3\,\pi }{2}\right)\newline -N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{3\,\pi }{2},-\frac{\pi }{2}\right)\newline 0 & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{\pi }{2},\frac{\pi }{2}\right)\newline -N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{\pi }{2},\frac{3\,\pi }{2}\right)\newline 0 & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{3\,\pi }{2},\frac{5\,\pi }{2}\right)\newline -N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{5\,\pi }{2},\frac{7\,\pi }{2}\right)\newline 0 & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{7\,\pi }{2},4\,\pi \right) \end{array}\right.$
 

```matlab

statorCountingFunction_ext_avg = 1/T * int(statorCountingFunction_ext, phi, [0 T]);
statorWindingFunction_ext(phi) = statorCountingFunction_ext - statorCountingFunction_ext_avg
```
statorWindingFunction_ext(phi) = 
 $\displaystyle \left\lbrace \begin{array}{cl} \frac{5734161139222659\,\pi \,N_s }{36028797018963968} & \;\textrm{if}\;\;\phi \in \left\lbrack -4\,\pi ,-\frac{7\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968}-N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{7\,\pi }{2},-\frac{5\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968} & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{5\,\pi }{2},-\frac{3\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968}-N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{3\,\pi }{2},-\frac{\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968} & \;\textrm{if}\;\;\phi \in \left\lbrack -\frac{\pi }{2},\frac{\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968}-N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{\pi }{2},\frac{3\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968} & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{3\,\pi }{2},\frac{5\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968}-N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{5\,\pi }{2},\frac{7\,\pi }{2}\right)\newline \frac{5734161139222659\,\pi \,N_s }{36028797018963968} & \;\textrm{if}\;\;\phi \in \left\lbrack \frac{7\,\pi }{2},4\,\pi \right) \end{array}\right.$
 

```matlab

figure 
fplot(subs(statorCountingFunction(phi), N_s, NS), [-2*T 2*T], ...
    "DisplayName", "Original Stator Counting Function", ...
    "LineWidth", 1.2)
hold on 
fplot(subs(statorCountingFunction_ext(phi), N_s, NS), [-2*T 2*T], ...
    "DisplayName", "Extended Stator Counting Function", ...
    "LineWidth", 1.2)
fplot(subs(statorWindingFunction_ext(phi), N_s, NS), [-2*T 2*T], ...
    "DisplayName", "Extended Stator Winding Function", ...
    "LineWidth", 1.2)
hold off 
ylim([-2 2])
grid on  
legend 

ax = gca;
S = sym(ax.XLim(1):pi/2:ax.XLim(2));
ax.XTick = double(S);
ax.XTickLabel = arrayfun(@texlabel,S,'UniformOutput',false);
```

![](./ECEN611_HW4_Problem_3_media//)

## Rotor Counting & Winding Function 
```matlab
syms N_r 
NR = 1;

turnsRotorNum = [0     1    -1    0 ] * N_r;   % number of turns at each location: replace Nt with 1 for simplicity  
turnsRotorPhi = [0   T/4   T/2  T/4 ];  % angle of each location

turnsRotorNumLevel = cumsum(turnsRotorNum)
```
turnsRotorNumLevel = 
 $\displaystyle \left(\begin{array}{cccc} 0 & N_r  & 0 & 0 \end{array}\right)$
 

```matlab

rotor_winding_turning_phi = theta_r + cumsum(turnsRotorPhi)
```
rotor_winding_turning_phi = 
 $\displaystyle \left(\begin{array}{cccc} \theta_r  & \theta_r +\frac{\pi }{2} & \theta_r +\frac{3\,\pi }{2} & \theta_r +2\,\pi  \end{array}\right)$
 

```matlab
rotorCountingFunction(phi) = turnsRotorNumLevel(1);

phiReference = 0;

for k = 1:length(rotor_winding_turning_phi)-1
    
    thisPhi = rotor_winding_turning_phi(k);
    nextPhi = rotor_winding_turning_phi(k+1);

    rotorCountingFunction(phi) = piecewise(thisPhi <= phi < nextPhi, turnsRotorNumLevel(k), ...
                                           phiReference <= phi < thisPhi, rotorCountingFunction);
end 
```

### Original Rotor Counting Function φ ∈ \[θr θr+T\]
```matlab
disp("Original Rotor Counting Function: ");
```

```matlabTextOutput
Original Rotor Counting Function: 
```

```matlab
disp(rotorCountingFunction(phi));
```
 $\displaystyle \left\lbrace \begin{array}{cl} 0 & \;\textrm{if}\;\;2\,\theta_r +3\,\pi \le 2\,\phi \wedge \phi <\theta_r +2\,\pi \newline N_r  & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +3\,\pi \wedge 2\,\theta_r +\pi \le 2\,\phi \newline 0 & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +\pi \wedge 0\le \phi \wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \right)}\right)} \end{array}\right.$
 

### Extended Rotor Counting Function φ ∈ \[θr\-2T θr+2T\]
```matlab
rotorCountingFunction_ext(phi) = piecewise( ...
    theta_r - 2*T <= phi < theta_r - T, rotorCountingFunction(phi+2*T), ...
    theta_r - T <= phi < theta_r,       rotorCountingFunction(phi+T), ...
    theta_r <= phi < theta_r + T,       rotorCountingFunction(phi), ...
    theta_r + T <= phi < theta_r + 2*T, rotorCountingFunction(phi-T) ...
)
```
rotorCountingFunction_ext(phi) = 
 $\displaystyle \left\lbrace \begin{array}{cl} 0 & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +5\,\pi \wedge \phi +2\,\pi <\theta_r \newline N_r  & \;\textrm{if}\;\;2\,\phi +5\,\pi <2\,\theta_r \wedge 2\,\theta_r \le 2\,\phi +7\,\pi \wedge \phi \in \mathbb{R}\newline 0 & \;\textrm{if}\;\;{\left(\phi <\theta_r \wedge 2\,\theta_r \le 2\,\phi +\pi \right)}\vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +4\,\pi <\theta_r \vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \right)}\right)}\right)}\newline N_r  & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +3\,\pi \wedge \phi \in \mathbb{R}\wedge 2\,\phi +\pi <2\,\theta_r \newline 0 & \;\textrm{if}\;\;{\left(2\,\theta_r +3\,\pi \le 2\,\phi \wedge \phi <\theta_r +2\,\pi \right)}\vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +2\,\pi <\theta_r \vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \right)}\right)}\right)}\newline N_r  & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +3\,\pi \wedge \phi \in \mathbb{R}\wedge 2\,\theta_r +\pi \le 2\,\phi \newline 0 & \;\textrm{if}\;\;{\left(2\,\theta_r +7\,\pi \le 2\,\phi \wedge \phi <\theta_r +4\,\pi \right)}\vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \right)}\right)}\right)}\newline N_r  & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +7\,\pi \wedge 2\,\theta_r +5\,\pi \le 2\,\phi \wedge \phi \in \mathbb{R}\newline 0 & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +5\,\pi \wedge \theta_r +2\,\pi \le \phi \wedge \phi \in \mathbb{R}\wedge {\left(\phi <\theta_r +2\,\pi \vee {\left(2\,\phi <2\,\theta_r +5\,\pi \wedge \theta_r +2\,\pi \le \phi \right)}\right)} \end{array}\right.$
 

### Extended Rotor Winding Function φ ∈ \[θr\-2T θr+2T\]
```matlab
% assume( theta < T )
rotorCountingFunction_ext_avg = 1/T * int(rotorCountingFunction_ext, phi, [0 T]);
rotorWindingFunction_ext = rotorCountingFunction_ext - rotorCountingFunction_ext_avg
```
rotorWindingFunction_ext(phi) = 
 $\displaystyle \left\lbrace \begin{array}{cl} \frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968}-\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +5\,\pi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge \phi +2\,\pi <\theta_r \newline -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +5\,\pi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge \phi +2\,\pi <\theta_r \newline N_r -\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968}+\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\phi +5\,\pi <2\,\theta_r \wedge 2\,\theta_r \le 2\,\phi +7\,\pi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge \phi \in \mathbb{R}\newline N_r -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\phi +5\,\pi <2\,\theta_r \wedge 2\,\theta_r \le 2\,\phi +7\,\pi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge \phi \in \mathbb{R}\newline \frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968}-\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;\theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge {\left({\left(\phi <\theta_r \wedge 2\,\theta_r \le 2\,\phi +\pi \right)}\vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +4\,\pi <\theta_r \vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \right)}\right)}\right)}\right)}\newline -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;{\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge {\left({\left(\phi <\theta_r \wedge 2\,\theta_r \le 2\,\phi +\pi \right)}\vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +4\,\pi <\theta_r \vee {\left(2\,\phi +7\,\pi <2\,\theta_r \wedge \theta_r \le \phi +4\,\pi \right)}\right)}\right)}\right)}\newline N_r -\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968}+\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +3\,\pi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge \phi \in \mathbb{R}\wedge 2\,\phi +\pi <2\,\theta_r \newline N_r -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\theta_r \le 2\,\phi +3\,\pi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge \phi \in \mathbb{R}\wedge 2\,\phi +\pi <2\,\theta_r \newline \frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968}-\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;\theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge {\left({\left(2\,\theta_r +3\,\pi \le 2\,\phi \wedge \phi <\theta_r +2\,\pi \right)}\vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +2\,\pi <\theta_r \vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \right)}\right)}\right)}\right)}\newline -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;{\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge {\left({\left(2\,\theta_r +3\,\pi \le 2\,\phi \wedge \phi <\theta_r +2\,\pi \right)}\vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \wedge \phi \in \mathbb{R}\wedge {\left(\phi +2\,\pi <\theta_r \vee {\left(2\,\phi +3\,\pi <2\,\theta_r \wedge \theta_r \le \phi +2\,\pi \right)}\right)}\right)}\right)}\newline N_r -\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968}+\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +3\,\pi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge 2\,\theta_r +\pi \le 2\,\phi \newline N_r -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +3\,\pi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge 2\,\theta_r +\pi \le 2\,\phi \newline \frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968}-\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;\theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge {\left({\left(2\,\theta_r +7\,\pi \le 2\,\phi \wedge \phi <\theta_r +4\,\pi \right)}\vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \right)}\right)}\right)}\right)}\newline -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;{\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge {\left({\left(2\,\theta_r +7\,\pi \le 2\,\phi \wedge \phi <\theta_r +4\,\pi \right)}\vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2\,\phi <2\,\theta_r +\pi \right)}\right)}\right)}\right)}\newline N_r -\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968}+\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +7\,\pi \wedge 2\,\theta_r +5\,\pi \le 2\,\phi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\newline N_r -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +7\,\pi \wedge 2\,\theta_r +5\,\pi \le 2\,\phi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\newline \frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{3\,\pi }{2}\right)}}{36028797018963968}-\frac{5734161139222659\,N_r \,{\left(\theta_r -\frac{\pi }{2}\right)}}{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +5\,\pi \wedge \theta_r \in \left(\frac{\pi }{2},\frac{3\,\pi }{2}\right)\wedge \theta_r +2\,\pi \le \phi \wedge {\left(\phi <\theta_r +2\,\pi \vee {\left(2\,\phi <2\,\theta_r +5\,\pi \wedge \theta_r +2\,\pi \le \phi \right)}\right)}\newline -\frac{5734161139222659\,\pi \,N_r }{36028797018963968} & \;\textrm{if}\;\;2\,\phi <2\,\theta_r +5\,\pi \wedge {\left(\theta_r \in \left\lbrack \frac{3\,\pi }{2},2\,\pi \right\rbrack \vee \theta_r \le \frac{\pi }{2}\right)}\wedge \theta_r +2\,\pi \le \phi \wedge {\left(\phi <\theta_r +2\,\pi \vee {\left(2\,\phi <2\,\theta_r +5\,\pi \wedge \theta_r +2\,\pi \le \phi \right)}\right)} \end{array}\right.$
 

```matlab

figure 
fplot(subs(rotorCountingFunction_ext(phi),[theta_r N_r], [0 NR]), [-2*T 2*T], ...
    "DisplayName", "Extended Rotor Counting Function", ...
    "LineWidth", 1.2)
hold on 
fplot(subs(rotorWindingFunction_ext(phi),[theta_r N_r], [0 NR]), [-2*T 2*T], ...
    "DisplayName", "Extended Rotor Winding Function", ...
    "LineWidth", 1.2)
hold off 
ylim([-2 2])
grid on  
legend 
```

![](./ECEN611_HW4_Problem_3_media//)

## Stator Winding Magnetizing Inductance
```matlab
syms mu_o r l g 
Lss = mu_o*r*l/g * int( statorWindingFunction_ext * statorWindingFunction_ext, ...
                     phi, [0 T]);

Lss_simplified = subs( simplify( vpa(Lss) ) )
```
Lss_simplified = 
 $\displaystyle \frac{1.571\,{N_s }^2 \,l\,\mu_o \,r}{g}$
 

```matlab
% Lss_simplified = subs( simplify( vpa(Lss) ), N_s^2*l*mu_o*r/g, 1 )
```

## Rotor Winding Magnetizing Inductance
```matlab
syms mu_o r l g 

Lrr_integrand = subs(rotorWindingFunction_ext * rotorWindingFunction_ext, theta_r, 0:T/8:T);

tic
Lrr = vpa( simplify( mu_o*r*l/g * int( Lrr_integrand, phi, [0 T] ) ) )
```
Lrr = 
 $\displaystyle \left(\begin{array}{ccccccccc} \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} & \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g} \end{array}\right)$
 

```matlab
toc 
```

```matlabTextOutput
Elapsed time is 1.354199 seconds.
```

```matlab

Lrr = Lrr(1)
```
Lrr = 
 $\displaystyle \frac{1.571\,{N_r }^2 \,l\,\mu_o \,r}{g}$
 

Rotor Winding Magnetizing Inductance is independent of rotor angle.

## Stator\-Rotor Winding Mutual Inductance 

![](./ECEN611_HW4_Problem_3_media//)

```matlab
Lsr_integrand = simplify( vpa( rotorWindingFunction_ext * statorWindingFunction_ext ) )
```
Lsr_integrand(phi) = 
 $\displaystyle \left\lbrace \begin{array}{cl} -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi <-4.712\wedge 1.571<\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +15.71\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi +6.283<\theta_r \wedge \theta_r <4.712\wedge 2.0\,\theta_r \le 2.0\,\phi +15.71\wedge \phi \in \left\lbrack -4.712,-1.571\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi +6.283<\theta_r \wedge \phi <-4.712\wedge 2.0\,\theta_r \le 2.0\,\phi +15.71\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi +6.283<\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +15.71\wedge \phi \in \left\lbrack -4.712,-1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi +6.283<\theta_r \wedge \phi \in \left\lbrack -1.571,1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;1.571<\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +21.99\wedge \phi <-7.854\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +21.99\wedge 2.0\,\phi +15.71<2.0\,\theta_r \wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -7.854,-4.712\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r <4.712\wedge 2.0\,\phi +15.71<2.0\,\theta_r \wedge \phi \in \left\lbrack -4.712,-1.571\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +21.99\wedge \phi <-7.854\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +21.99\wedge 2.0\,\phi +15.71<2.0\,\theta_r \wedge \phi \in \left\lbrack -7.854,-4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi +15.71<2.0\,\theta_r \wedge \phi \in \left\lbrack -4.712,-1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -12.57,-11.0\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -11.0,-7.854\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -7.854,-4.712\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -4.712,-1.571\right)\wedge \theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -1.571,1.571\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 1.571,4.712\right)\wedge \theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 4.712,7.854\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 7.854,11.0\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 11.0,12.57\right)\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -12.57,-11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -11.0,-7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -7.854,-4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -4.712,-1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -1.571,1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 1.571,4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 4.712,7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 7.854,11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 11.0,12.57\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\wedge {\left({\left(\phi <\theta_r \wedge 2.0\,\theta_r \le 2.0\,\phi +3.142\right)}\vee {\left(\theta_r \le \phi +12.57\wedge {\left(\phi +12.57<\theta_r \vee {\left(\theta_r \le \phi +12.57\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\wedge 2.0\,\phi +21.99<2.0\,\theta_r \right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +9.425\wedge \phi <-1.571\wedge 1.571<\theta_r \newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +9.425\wedge 2.0\,\phi +3.142<2.0\,\theta_r \wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -1.571,1.571\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r <4.712\wedge 2.0\,\phi +3.142<2.0\,\theta_r \wedge \phi \in \left\lbrack 1.571,4.712\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +9.425\wedge \phi <-1.571\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\theta_r \le 2.0\,\phi +9.425\wedge 2.0\,\phi +3.142<2.0\,\theta_r \wedge \phi \in \left\lbrack -1.571,1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi +3.142<2.0\,\theta_r \wedge \phi \in \left\lbrack 1.571,4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -12.57,-11.0\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -11.0,-7.854\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -7.854,-4.712\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -4.712,-1.571\right)\wedge \theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -1.571,1.571\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 1.571,4.712\right)\wedge \theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 4.712,7.854\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 7.854,11.0\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r \in \left(1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 11.0,12.57\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -12.57,-11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -11.0,-7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -7.854,-4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack -4.712,-1.571\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -1.571,1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi \in \left\lbrack 1.571,4.712\right)\wedge {\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 4.712,7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 7.854,11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +6.283\wedge 2.0\,\theta_r +9.425\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \wedge {\left(\phi +6.283<\theta_r \vee {\left(\theta_r \le \phi +6.283\wedge 2.0\,\phi +9.425<2.0\,\theta_r \right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 11.0,12.57\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi <4.712\wedge 1.571<\theta_r \wedge 2.0\,\theta_r +3.142\le 2.0\,\phi \newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi <2.0\,\theta_r +9.425\wedge 2.0\,\theta_r +3.142\le 2.0\,\phi \wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 4.712,7.854\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r <4.712\wedge 2.0\,\phi <2.0\,\theta_r +9.425\wedge \phi \in \left\lbrack 7.854,11.0\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi <4.712\wedge 2.0\,\theta_r +3.142\le 2.0\,\phi \wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi <2.0\,\theta_r +9.425\wedge 2.0\,\theta_r +3.142\le 2.0\,\phi \wedge \phi \in \left\lbrack 4.712,7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi <2.0\,\theta_r +9.425\wedge \phi \in \left\lbrack 7.854,11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -12.57,-11.0\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -11.0,-7.854\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -7.854,-4.712\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -4.712,-1.571\right)\wedge \theta_r \in \left(1.571,4.712\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack -1.571,1.571\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 1.571,4.712\right)\wedge \theta_r \in \left(1.571,4.712\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 4.712,7.854\right)\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 7.854,11.0\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \theta_r \in \left(1.571,4.712\right)\wedge \phi \in \left\lbrack 11.0,12.57\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -12.57,-11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -11.0,-7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -7.854,-4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -4.712,-1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack -1.571,1.571\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 1.571,4.712\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 4.712,7.854\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 7.854,11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;{\left({\left(\phi <\theta_r +12.57\wedge 2.0\,\theta_r +21.99\le 2.0\,\phi \right)}\vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\wedge {\left(\phi <\theta_r \vee {\left(\theta_r \le \phi \wedge 2.0\,\phi <2.0\,\theta_r +3.142\right)}\right)}\right)}\right)}\wedge \phi \in \left\lbrack 11.0,12.57\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi <11.0\wedge 1.571<\theta_r \wedge 2.0\,\theta_r +15.71\le 2.0\,\phi \newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;1.571<\theta_r \wedge 2.0\,\theta_r +15.71\le 2.0\,\phi \wedge \phi \in \left\lbrack 11.0,12.57\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\phi <11.0\wedge 2.0\,\theta_r +15.71\le 2.0\,\phi \wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;2.0\,\phi <2.0\,\theta_r +21.99\wedge 2.0\,\theta_r +15.71\le 2.0\,\phi \wedge \phi \in \left\lbrack 11.0,12.57\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r +6.283\le \phi \wedge \phi <11.0\wedge {\left(\phi <\theta_r +6.283\vee {\left(\theta_r +6.283\le \phi \wedge 2.0\,\phi <2.0\,\theta_r +15.71\right)}\right)}\wedge 1.571<\theta_r \wedge 2.0\,\phi <2.0\,\theta_r +15.71\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r <4.712\wedge {\left(\phi <\theta_r +6.283\vee {\left(\theta_r +6.283\le \phi \wedge 2.0\,\phi <2.0\,\theta_r +15.71\right)}\right)}\wedge 2.0\,\phi <2.0\,\theta_r +15.71\wedge \phi \in \left\lbrack 11.0,12.57\right)\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r +6.283\le \phi \wedge {\left(\phi <\theta_r +6.283\vee {\left(\theta_r +6.283\le \phi \wedge 2.0\,\phi <2.0\,\theta_r +15.71\right)}\right)}\wedge \phi <7.854\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline 0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r +6.283\le \phi \wedge {\left(\phi <\theta_r +6.283\vee {\left(\theta_r +6.283\le \phi \wedge 2.0\,\phi <2.0\,\theta_r +15.71\right)}\right)}\wedge 2.0\,\phi <2.0\,\theta_r +15.71\wedge \phi \in \left\lbrack 7.854,11.0\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)}\newline -0.25\,N_r \,N_s  & \;\textrm{if}\;\;\theta_r +6.283\le \phi \wedge {\left(\phi <\theta_r +6.283\vee {\left(\theta_r +6.283\le \phi \wedge 2.0\,\phi <2.0\,\theta_r +15.71\right)}\right)}\wedge 2.0\,\phi <2.0\,\theta_r +15.71\wedge \phi \in \left\lbrack 11.0,12.57\right)\wedge {\left(4.712\le \theta_r \vee \theta_r \in \left\lbrack 0,1.571\right\rbrack \right)} \end{array}\right.$
 

```matlab
% Lsr_integrand = vpa( rotorWindingFunction_ext * statorWindingFunction_ext )
```

```matlab

tic
Lsr = mu_o*r*l/g * int(Lsr_integrand, phi, [0 T]); 
toc
```

```matlabTextOutput
Elapsed time is 22.924358 seconds.
```

```matlab

Lsr_simplified = subs( simplify( vpa(Lsr) ), N_r*N_s*l*mu_o*r/g, 1 )
```
Lsr_simplified = 
 $\displaystyle \left\lbrace \begin{array}{cl} -1.571 & \;\textrm{if}\;\;\theta_r =0\newline -\text{9.183e-7} & \;\textrm{if}\;\;\theta_r =4.712\newline -\text{3.673e-6} & \;\textrm{if}\;\;\theta_r =4.712\newline 1.571 & \;\textrm{if}\;\;\theta_r =3.142\newline 1.571 & \;\textrm{if}\;\;\theta_r =3.142\newline 2.356-0.5\,\theta_r  & \;\textrm{if}\;\;\theta_r \in \left(4.712,4.712\right\rbrack \newline 0.75\,\theta_r -1.178 & \;\textrm{if}\;\;\theta_r \in \left\lbrack 1.571,1.571\right\rbrack \newline 1.571 & \;\textrm{if}\;\;\theta_r \in \left(3.142,3.142\right)\newline 4.712-1.0\,\theta_r  & \;\textrm{if}\;\;4.712<\theta_r \vee \theta_r \in \left(3.142,4.712\right)\newline 1.0\,\theta_r -1.571 & \;\textrm{if}\;\;\theta_r \in \left(0.0,1.571\right)\vee \theta_r \in \left(1.571,3.142\right) \end{array}\right.$
 

```matlab
figure 

fplot(Lsr_simplified, [0 T], ...
    "LineWidth", 1.2)
ylim([-3 3])
grid on  
title("Stator-Rotor Winding Mutual Inductance")

ax = gca;
S = sym(ax.XLim(1):pi/4:ax.XLim(2));
ax.XTick = double(S);
ax.XTickLabel = arrayfun(@texlabel,S,'UniformOutput',false);
```

![](./ECEN611_HW4_Problem_3_media//)

```matlab
% Fourier Series of Lsr 
digits(4)

a0 = vpa( (1/T) * int( Lsr_simplified, theta_r, [0 T], ...
    'IgnoreSpecialCases', true) )
```
a0 = 
 $\displaystyle \text{6.047e-5}$
 

```matlab
syms n 

an = (2/T) * int( Lsr_simplified * cos(n * theta_r), theta_r, [0 T]);
bn = (2/T) * int( Lsr_simplified * sin(n * theta_r), theta_r, [0 T]);

orderOfHarmonics = 1:13;
an_13 = vpa( simplify( subs(an, n, orderOfHarmonics) ) );
bn_13 = vpa( simplify( subs(bn, n, orderOfHarmonics) ) );

tolerance = 1e-4;

an_13(abs(an_13) < tolerance) = 0
```
an_13 = 
 $\displaystyle \left(\begin{array}{ccccccccccccc} -1.273 & 0 & -0.1415 & 0 & -0.05093 & 0 & -0.02599 & 0 & -0.01572 & 0 & -0.01052 & 0 & -0.007533 \end{array}\right)$
 

```matlab
bn_13(abs(bn_13) < tolerance) = 0
```
bn_13 = 
 $\displaystyle \left(\begin{array}{ccccccccccccc} 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \end{array}\right)$
 

```matlab

Lsr_fourier_filtered = a0 + ...
    sum(an_13 .* cos(orderOfHarmonics .* theta_r) + ...
        bn_13 .* sin(orderOfHarmonics .* theta_r))
```
Lsr_fourier_filtered = 
 $\displaystyle \text{6.047e-5}-0.05093\,\cos \left(5\,\theta_r \right)-0.02599\,\cos \left(7\,\theta_r \right)-0.01572\,\cos \left(9\,\theta_r \right)-0.01052\,\cos \left(11\,\theta_r \right)-0.007533\,\cos \left(13\,\theta_r \right)-1.273\,\cos \left(\theta_r \right)-0.1415\,\cos \left(3\,\theta_r \right)$
 

```matlab
Lsr_fourier_approx = an_13(1) * cos(orderOfHarmonics(1) * theta_r)
```
Lsr_fourier_approx = 
 $\displaystyle -1.273\,\cos \left(\theta_r \right)$
 

```matlab

figure 
fplot(Lsr_simplified, [0 T], ...
      "DisplayName", "Original Lsr(θr)", ...
      "LineWidth", 1.2)
hold on 
fplot(Lsr_fourier_filtered, [0 T], ...
      "DisplayName", "Complete Fourier Series", ...
      "LineStyle",":", ...
      "LineWidth", 1.2 ...
      )
fplot(Lsr_fourier_approx, [0 T], ...
      "DisplayName", "Approx. Fourier Series (Fundamental Only)", ...
      "LineStyle",":", ...
      "LineWidth", 1.2 ...
      )
hold off 
ylim([-3 3])
grid on 
legend 

ax = gca;
S = sym(ax.XLim(1):pi/2:ax.XLim(2));
ax.XTick = double(S);
ax.XTickLabel = arrayfun(@texlabel,S,'UniformOutput',false);
```

![](./ECEN611_HW4_Problem_3_media//)

## Torque Production
```matlab
syms I_s1 omega_1 I_s2 omega_2 
syms t 
syms phi_2 omega_r theta_r 

magnitude = [I_s1 I_s2];
omega = [omega_1 omega_2];
angle = [ omega_1*t omega_2*t + phi_2 ];
current = magnitude .* cos(angle);
i1 = current(1)
```
i1 = 
 $\displaystyle I_{\textrm{s1}} \,\cos \left(\omega_1 \,t\right)$
 

```matlab
i2 = current(2)
```
i2 = 
 $\displaystyle I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)$
 

### Reluctance Torque 
```matlab
reluctanceTorque = 1/2 * (i1^2*diff(Lss,theta_r) + i2^2*diff(Lrr,theta_r))
```
reluctanceTorque = 
 $\displaystyle 0.0$
 

### Alignment Torque 
```matlab
disp(Lsr_fourier_approx)
```
 $\displaystyle -1.273\,\cos \left(\theta_r \right)$
 

```matlab
alignmentTorque = i1 * i2 * diff(Lsr_fourier_approx, theta_r)
```
alignmentTorque = 
 $\displaystyle 1.273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)\,\cos \left(\omega_1 \,t\right)\,\sin \left(\theta_r \right)$
 

### Total Torque
```matlab
totalTorque = simplify( reluctanceTorque + alignmentTorque )
```
totalTorque = 
 $\displaystyle 1.273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)\,\cos \left(\omega_1 \,t\right)\,\sin \left(\theta_r \right)$
 

```matlab
syms omega_r omega_r0 omega_m delta
totalTorque = subs(totalTorque, theta_r, omega_m*t + delta)
```
totalTorque = 
 $\displaystyle 1.273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)\,\sin \left(\delta +\omega_m \,t\right)\,\cos \left(\omega_1 \,t\right)$
 


 ![](./ECEN611_HW4_Problem_3_media//)


Given the transformation above, it is clear that the average value of each term is zero unless the coefficient of t is zero. 


Unfortunately, MATLAB seems incapable of performing such a transformation using Product\-to\-Sum Trigonometric Identities


cos(A)cos(B)=1/2�\[cos(A+B)+cos(A−B)\]


sin(A)cos(B)=1/2�\[sin(A+B)+sin(A−B)\]


as you can see if you uncomment the following code.

```matlab
% expanded_totalTorque = expand(totalTorque)
% rewritten_totalTorque = rewrite(expanded_totalTorque, 'sin')
% simplified_totalTorque = simplify(rewritten_totalTorque)
% final_totalTorque = expand(simplified_totalTorque)
```

Although, MATLAB can do this the other way round:

```matlab
syms A B
ccSum = 1/2*(cos(A+B)+cos(A-B))
```
ccSum = 
 $\displaystyle \frac{\cos \left(A+B\right)}{2}+\frac{\cos \left(A-B\right)}{2}$
 

```matlab
ccProduct = simplify(ccSum)
```
ccProduct = 
 $\displaystyle \cos \left(A\right)\,\cos \left(B\right)$
 

```matlab
ssSum = 1/2*(sin(A+B)+sin(A-B))
```
ssSum = 
 $\displaystyle \frac{\sin \left(A+B\right)}{2}+\frac{\sin \left(A-B\right)}{2}$
 

```matlab
ssProduct = simplify(ssSum)
```
ssProduct = 
 $\displaystyle \cos \left(B\right)\,\sin \left(A\right)$
 

Well, let us realize it manually:

```matlab
% Define the four sine terms
term1 = sin((omega_m + (omega_1 + omega_2))*t + phi_2 + delta);
term2 = sin((omega_m - (omega_1 + omega_2))*t - phi_2 + delta);
term3 = sin((omega_m + (omega_1 - omega_2))*t - phi_2 + delta);
term4 = sin((omega_m - (omega_1 - omega_2))*t + phi_2 + delta);

% Combine them with the coefficient
totalTorque_manual = (1.273 * I_s1 * I_s2 / 4) * (term1 + term2 + term3 + term4);

% Simplify and verify 
simplify(vpa(totalTorque_manual))
```
ans = 
 $\displaystyle 1.273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)\,\sin \left(\delta +\omega_m \,t\right)\,\cos \left(\omega_1 \,t\right)$
 

```matlab
vpa(totalTorque)
```
ans = 
 $\displaystyle 1.273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\cos \left(\phi_2 +\omega_2 \,t\right)\,\sin \left(\delta +\omega_m \,t\right)\,\cos \left(\omega_1 \,t\right)$
 

## Rotor Speed ωr

![](./ECEN611_HW4_Problem_3_media//)


Determine the rotor speeds at which the device produces a nonzero average torque during steady state operation if

### (a) When ω1 = ω2 = 0

DC current supply. Single phase machine.

```matlab
totalTorque_case_a = subs(totalTorque_manual, omega, [0 0])
```
totalTorque_case_a = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(2\,\sin \left(\delta -\phi_2 +\omega_m \,t\right)+2\,\sin \left(\delta +\phi_2 +\omega_m \,t\right)\right)}}{4000}$
 

```matlab
totalTorque_case_a = subs(totalTorque_case_a, omega_m, 0)
```
totalTorque_case_a = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(2\,\sin \left(\delta -\phi_2 \right)+2\,\sin \left(\delta +\phi_2 \right)\right)}}{4000}$
 

```matlab
% totalTorque_case_a_avg = 1/T * int( ...
%     subs(totalTorque_case_a, omega_r*t, theta_r), ...
%     theta_r, [0 T])
```

Only when $\omega_m =0$ can the device produce a nonzero average torque. 

### (b) When ω1 = ω2 ≠ 0

Let's assume ω1 = ω2 = ω1

```matlab
totalTorque_case_b = subs(totalTorque_manual, omega(2), omega_1)
```
totalTorque_case_b = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(\sin \left(\delta +\phi_2 +t\,{\left(2\,\omega_1 +\omega_m \right)}\right)+\sin \left(\delta -\phi_2 +\omega_m \,t\right)-\sin \left(\phi_2 -\delta +t\,{\left(2\,\omega_1 -\omega_m \right)}\right)+\sin \left(\delta +\phi_2 +\omega_m \,t\right)\right)}}{4000}$
 

```matlab
% totalTorque_case_b_avg = simplify( ...
%     expand( ...
%         vpa( 1/T * int(totalTorque_case_b, t, [0 T/omega_m]) ) ...
%     ) ...
% );
```

Either 

 $$ \begin{array}{l} 2\,\omega_1 +\omega_m =0\Rightarrow \omega_m =-2\omega_1 \;\newline \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\textrm{and}\;\newline \;\;\;\;\;\;\;\delta -\phi_2 \not= 0\Rightarrow \delta \not= \phi_2 \; \end{array} $$ 
```matlab
totalTorque_case_b_nonZero = simplify(subs(totalTorque_case_b, omega_m, 2*omega_1))
```
totalTorque_case_b_nonZero = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(\sin \left(\delta -\phi_2 \right)+\sin \left(\delta -\phi_2 +2\,\omega_1 \,t\right)+\sin \left(\delta +\phi_2 +2\,\omega_1 \,t\right)+\sin \left(\delta +\phi_2 +4\,\omega_1 \,t\right)\right)}}{4000}$
 

or 

 $$ \begin{array}{l} \omega_m =0\;\textrm{and}\;\newline \delta \not= 0,\;\phi_2 \not= 0\; \end{array} $$ 
```matlab
totalTorque_case_b_nonZero = simplify(subs(totalTorque_case_b, omega_m, 0))
```
totalTorque_case_b_nonZero = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,\sin \left(\delta \right)\,{\left(\cos \left(\phi_2 +2\,\omega_1 \,t\right)+\cos \left(\phi_2 \right)\right)}}{2000}$
 

will result in a non\-zero torque.

### (c) When ω1 ≠ 0, ω2 = 0
```matlab
totalTorque_case_c = subs(totalTorque_manual, omega(2), 0)
```
totalTorque_case_c = 
 $\displaystyle \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(\sin \left(\delta -\phi_2 +t\,{\left(\omega_1 +\omega_m \right)}\right)+\sin \left(\delta +\phi_2 -t\,{\left(\omega_1 -\omega_m \right)}\right)-\sin \left(\phi_2 -\delta +t\,{\left(\omega_1 -\omega_m \right)}\right)+\sin \left(\delta +\phi_2 +t\,{\left(\omega_1 +\omega_m \right)}\right)\right)}}{4000}$
 

```matlab
totalTorque_case_c_nonZero = subs(totalTorque_case_c, omega_m, [omega_1 -omega_1])
```
totalTorque_case_c_nonZero = 
 $\displaystyle \left(\begin{array}{cc} \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(\sin \left(\delta -\phi_2 \right)+\sin \left(\delta -\phi_2 +2\,\omega_1 \,t\right)+\sin \left(\delta +\phi_2 \right)+\sin \left(\delta +\phi_2 +2\,\omega_1 \,t\right)\right)}}{4000} & \frac{1273\,I_{\textrm{s1}} \,I_{\textrm{s2}} \,{\left(\sin \left(\delta -\phi_2 \right)-\sin \left(\phi_2 -\delta +2\,\omega_1 \,t\right)+\sin \left(\delta +\phi_2 \right)+\sin \left(\delta +\phi_2 -2\,\omega_1 \,t\right)\right)}}{4000} \end{array}\right)$
 

Only when $\omega_m =|\omega_1 |$ the device can produce a non\-zero torque.

