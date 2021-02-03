# E2G_Technical_Interview_Problem

Thermal Model Code Matlab

thermalmodel = createpde('thermal','transient-axisymmetric');
pderect([0 2.82 0 0.188])
pderect([0.94 2.82 0.190 0.378])
pderect([0.94 0.95 0.188 0.190])
gd = [pderect pderect pderect];
sf = 'pderect+pderect+pderect';
ns = char('pderect','pderect','pderect');
ns = ns';
d1 = descg(gd,sf,ns);
geometryFromEdges(thermalmodel,d1);
thermalProperties(thermalmodel,'ThermalConductivity',31.95,'MassDensity',0.284,'SpecificHeat',0.119);
thermalBC(thermalmodel,'Edge',[2,3,4,5,6,8],'HeatFlux',0);
thermalBC(thermalmodel,'Edge',1,'ConvectionCoefficient',48,'ProcessTemperature',325);
thermalBC(thermalmodel,'Edge',7,'ConvectionCoefficient',9,'AmbientTemperature',70);
thermalIC(thermalmodel,0);
internalHeatSource(thermalModel,@heatsource, 'Edge',1)
generateMesh(thermalmodel);

tfinal = 10;
tlist = 0:0.01:tfinal;
result = solve(thermalmodel,tlist);
T = result.Temperature;

figure 
subplot(2,1,1)
pdeplot(thermalmodel,'XYData',T(:,6),'Contour','on')
axis equal
title(sprintf('Temperature at %g s',tlist(6)))
subplot(2,1,2)
pdeplot(thermalmodel,'XYData',T(:,end),'Contour','on')
axis equal
title(sprintf('Temperature at %g s',tfinal))


internalHeatSource(thermalModel,@heatsource, 'Edge',4)
function q = heatsource(location,state)
t  = state.time;
x = location.x;
k = 31.95;
q = 2700*x.*(0.5+0.5*cos((k+5)*(t/5)));
end
