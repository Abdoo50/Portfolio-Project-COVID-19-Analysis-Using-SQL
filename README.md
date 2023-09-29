# Portfolio Project: COVID-19 Analysis

This project focuses on analyzing COVID-19 data to gain insights into the impact of the pandemic. It includes SQL queries to retrieve and analyze data related to cases, deaths, vaccination rates, and more.

## Getting Started

To run these queries, you need access to the PortfolioProject database. Make sure you have the necessary permissions to execute SQL queries.

## Queries

### Query 1: Total Cases and Deaths by Continent

This query retrieves the total cases and total deaths for each continent.

```sql
SELECT continent, Max(cast(Total_deaths as int)) as TotalDeathCount
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC
```

### Query 2: Total Cases and Deaths by Country

This query retrieves the total cases and total deaths for each country.

```sql
SELECT Location, Max(cast(Total_deaths as int)) as TotalDeathCount
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY Location
ORDER BY TotalDeathCount DESC
```

### Query 3: Total Cases and Deaths in United States

This query retrieves the total cases and total deaths in the United States.

```sql
SELECT Location, date, total_cases, total_deaths, (CAST(total_deaths AS float) / CAST(total_cases AS float)) * 100 AS DeathPercentage
FROM PortfolioProject..CovidDeaths
WHERE location LIKE '%states%' AND continent IS NOT NULL
ORDER BY Location, date
```

### Query 4: Total Cases vs Population

This query shows the percentage of the population that has been infected by COVID-19.

```sql
SELECT Location, date, total_cases, population, (CAST(total_cases AS float) / CAST(population AS float)) * 100 AS InfectionPercentage
FROM PortfolioProject..CovidDeaths
ORDER BY Location, date
```

### Query 5: Countries with Highest Infection Rate to Population

This query identifies the countries with the highest infection rate relative to their population.

```sql
SELECT Location, population, Max(total_cases) as HighestInfectionCount, Max((CAST(total_cases AS float) / CAST(population AS float))) * 100 AS PercentagePopulationInfected
FROM PortfolioProject..CovidDeaths
GROUP BY Location, population
ORDER BY PercentagePopulationInfected DESC
```

### Query 6: Total Population vs Vaccinations

This query shows the percentage of the population that has received at least one COVID-19 vaccine.

```sql
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(CONVERT(int,vac.new_vaccinations)) OVER (PARTITION BY dea.Location ORDER BY dea.location, dea.Date) as RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
JOIN PortfolioProject..CovidVaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
```

## Views

### PercentPopulationVaccinated

This view stores the data related to the percentage of the population that has been vaccinated against COVID-19. It includes the continent, location, date, population, new vaccinations, and rolling people vaccinated.

```sql
CREATE VIEW PercentPopulationVaccinated AS
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(CONVERT(int,vac.new_vaccinations)) OVER (PARTITION BY dea.Location ORDER BY dea.location, dea.Date) as RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
JOIN PortfolioProject..CovidVaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
```

## Conclusion

These queries and views provide valuable insights into the COVID-19 pandemic, including the total cases and deaths by continent and country, the infection rate relative to population, and the percentage of the population vaccinated. You can use this information to track the impact of the pandemic and make data-driven decisions.
