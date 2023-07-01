
## Requirements ##

- java 17+
- MySQL as database

## Dependency ##

__Maven__

    <dependency>
        <groupId>org.makechtec.software</groupId>
        <artifactId>sql_support</artifactId>
        <version>1.3.0</version>
    </dependency>

__Gradle__

    implementation 'org.makechtec.software:sql_support:1.3.0'

## Usage ##

__Example producing a Dto record__

    var connectionCredentials = new ConnectionInformation(
                "user",
                "pass",
                "host",
                "3306",
                "database"
        );

    ProducerByCall<Dto> producer =
                resultSet -> {

                    Dto dto = null;
                    while(resultSet.next()){
                        dto = new Dto(
                                resultSet.getInt("id"),
                                resultSet.getString("name")
                        );
                    }

                    return dto;
                };

    var dto =
            ProducerCallEngine.<Dto>builder(connectionCredentials)
                                .isPrepared()
                                .setQueryString("CALL dto_by_id(?)")
                                .addParamAtPosition(1, 1, ParamType.TYPE_INTEGER)
                                .produce(producer);

    assertFalse(dto.name().isEmpty());

Dto.java

    public record Dto(int id, String name) {}
