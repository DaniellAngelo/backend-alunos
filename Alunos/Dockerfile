# Etapa 1: build
# imagem oficial do maven com java 17 usada por compatibilidade com a versão do spring que escolhi
# nomeada como build
FROM maven:3.9.4-eclipse-temurin-17 AS build
#faz de /app diretorio de trabalho
WORKDIR /app
#copia pom.xml pois é arquivo de configuração que muda com menos frequencia o que atende ao requisito de otimização
COPY pom.xml .
#baixa as dependencias do projeto e é reutilizada até que o pom.xml mude
RUN mvn dependency:go-offline
#copia o código fonte após as dependências
COPY src ./src
#gera o jar e ignora a execução de testes durante a construção
RUN mvn clean package -DskipTests

# Etapa 2: imagem final
#Utiliza a imagem oficial do Eclipse Temurin (JDK 17) como base, mais leve que a do maven
FROM eclipse-temurin:17-jdk
#faz de /app diretorio de trabalho
WORKDIR /app
#copia o jar gerado na etapa anterior
COPY --from=build /app/target/*.jar app.jar
#define que a porta usada na aplicacao será 8080
EXPOSE 8080
#disponibiliza os comandos de execucao do app
ENTRYPOINT ["java", "-jar", "app.jar"]