select *
from PortfolioProject..CovidDeaths
where continent is not null
order by 3,4

--select *
--from PortfolioProject..CovidVaccinations
--order by 3,4

-- select data that we are going to be using

select location, date, total_cases, new_cases, total_deaths, population
from PortfolioProject..CovidDeaths
where continent is not null
order by 1,2

-- looking at total cases vs total deaths
-- shows likelihood of dying if you contract covid in your country
select location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
from PortfolioProject..CovidDeaths
where location like '%states%'
and continent is not null
order by 1,2

-- looking as total cases vs population
-- shows what percentage of population got covid
select location, date,population, total_cases, (total_cases/population)*100 as PercentagepopulationInfected
from PortfolioProject..CovidDeaths
--where location like '%states%'
order by 1,2

-- looking at Countries with Highest Infection Rate Compared to Population
select location, population,max (total_cases) as HighestInfectionCount, max((total_cases/population))*100 as PercentagepopulationInfected
from PortfolioProject..CovidDeaths
--where location like '%states%'
group by location, population
order by PercentagepopulationInfected desc

-- showing countries with highest death count per population

select location, max(cast(Total_deaths as int)) as TotalDeathCount
from PortfolioProject..CovidDeaths
--where location like '%states%'
where continent is not null
group by location
order by TotalDeathCount desc

-- lets break things down by continent
-- showing continents with the highest death count per population

select continent, max(cast(Total_deaths as int)) as TotalDeathCount
from PortfolioProject..CovidDeaths
--where location like '%states%'
where continent is  null
group by continent
order by TotalDeathCount desc

--global number

select sum (new_cases)as total_cases, sum(cast(new_deaths as int))as atotal_deaths,sum(cast(new_deaths as int))/sum(new_cases)*100 as DeathPercentage
from PortfolioProject..CovidDeaths
--where location like '%states%'
where continent is not null
--group by date
order by 1,2

-- looking at total population vs vaccinations

select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations ,
sum(convert(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location,dea.date)
as RollingpeopleVaccinated 
--, (RollingpeopleVaccinated/population)*100 
from PortfolioProject..CovidDeaths dea
join PortfolioProject..CovidVaccinations vac
     on dea.location = vac.location
     and dea.date = vac.date
     where dea.continent is not null
     order by 2,3
    

   -- USE CTE

   with popvsvac  ( continent , location, date, population, new_vaccinations, rollingpeoplevaccinated)
   as
   (
select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations ,
sum(convert(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location,dea.date)
as RollingpeopleVaccinated 
--, (RollingpeopleVaccinated/population)*100 
from PortfolioProject..CovidDeaths dea
join PortfolioProject..CovidVaccinations vac
     on dea.location = vac.location
     and dea.date = vac.date
     where dea.continent is not null
     --order by 2,3
     )
     select *, (rollingpeoplevaccinated/population)*100
     from popvsvac



     -- TEMP TABLE

     drop table if exists #percentpopulationvaccinated
     create table #percentpopulationvaccinated
     (
     continent nvarchar (255),
     location  nvarchar (255),
     date datetime,
     population numeric,
     new_vaccinations numeric,
     rollingpeoplevaccinated numeric,
     )
     insert into #percentpopulationvaccinated
     select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations ,
sum(convert(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location,dea.date)
as RollingpeopleVaccinated 
--, (RollingpeopleVaccinated/population)*100 
from PortfolioProject..CovidDeaths dea
join PortfolioProject..CovidVaccinations vac
     on dea.location = vac.location
     and dea.date = vac.date
    -- where dea.continent is not null
     --order by 2,3

      select *, (rollingpeoplevaccinated/population)*100
      from #percentpopulationvaccinated


      -- CREATING VIEW TO STORE DATA FOR LATER VISUALIZATION
      create view percentpopulationvaccinatedm as
       select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations ,
sum(convert(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location,dea.date)
as RollingpeopleVaccinated 
--, (RollingpeopleVaccinated/population)*100 
from PortfolioProject..CovidDeaths dea
join PortfolioProject..CovidVaccinations vac
     on dea.location = vac.location
     and dea.date = vac.date
    where dea.continent is not null
     --order by 2,3



   
