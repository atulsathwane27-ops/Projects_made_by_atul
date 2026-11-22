D = 0.1; L = 100; rho = 1000; mu = 0.01;
Q = linspace(0.001, 0.05, 100);

names = {'commercial steel', 'stainless steeel', 'cast iron','PVC'};
rough = [1.5e-5, 4.5e-2, 2.5e-6, 5.6e-2];
yield = [240, 45, 60, 15];

A = pi * D^2 /4;
V = Q /A ;
Re= rho * V* D / mu;

figure ; hold on;
for m = 1: length (names)
dP = zeros(size(Q));
  for k = 1: length (Q)
  f = friction_factor(Re(k),rough(m),D);
dP (k)= f * L/D * (rho * V(k)^2 /2 );
  end 
plot( Q, dP/1000 , 'LineWidth', 5, 'DisplayName',names{m});
fprintf('%s: %.1f kPa at max flow\n', names{m}, dP(end)/1000);
end
hold off
xlabel('flow rate(m^3)/s');
ylabel('pressure drop in (kPa)');
title('pressure drop for different pipe material');
legend('Location','northwest');
grid on;
P_op= 5e6;
SF = 2;
fprintf ('\n Minimum  wall thickness at %.0f Mpa:\n',P_op/1e6);
for m = 1: length(names)
S = yield(m)*1e6/ SF ;
t = P_op * D/ (2*S) ;
fprintf ('%S : %.2f mm\n', names{m}, t*1000);
end 
function f= friction_factor(Re, rough, D) 
if Re <2300 

f = 64/Re ;
else
   f = 0.25 / (log10(rough/(3.7*D) + 5.74/Re^0.9))^2;

end 
end

