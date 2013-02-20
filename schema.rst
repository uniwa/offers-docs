Offers XML Schema
=================

XSD
---

::

    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
        xmlns:tns="http://www.coupons.teiath.gr/schemas"
        targetNamespace="http://www.coupons.teiath.gr/schemas">

        <!-- Email type -->
        <xs:simpleType name="email_type">
            <xs:restriction base="xs:string">
                <xs:pattern value="\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*"/>
            </xs:restriction>
        </xs:simpleType>

        <!-- AFM -->
        <xs:simpleType name="afm_type">
            <xs:restriction base="xs:string">
                <xs:pattern value="\d{10}"/>
            </xs:restriction>
        </xs:simpleType>

        <!-- Postal Code -->
        <xs:simpleType name="postal_code_type">
            <xs:restriction base="xs:string">
                <xs:pattern value="\d{5}"/>
            </xs:restriction>
        </xs:simpleType>

        <!-- Photo -->
        <xs:simpleType name="photo_type">
            <xs:restriction base="xs:base64Binary"/>
        </xs:simpleType>

        <!-- An envelop for photos -->
        <xs:complexType name="array_of_photos_type">
            <xs:sequence>
                <xs:element name="photo" type="tns:photo_type" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Coordinations -->
        <xs:complexType name="coordinations_type">
            <xs:sequence>
                <xs:element name="latitude" type="xs:double"/>
                <xs:element name="longtitude" type="xs:double"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Address -->
        <xs:complexType name="address_type">
            <xs:sequence>
                <xs:element name="road" type="xs:string"/>
                <xs:element name="postal_code" type="tns:postal_code_type"/>
                <xs:element name="city" type="xs:string"/>
                <xs:element name="coordinations" type="tns:coordinations_type" minOccurs="0"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Day enumeration -->
        <xs:simpleType name="day_type">
            <xs:restriction base="xs:string">
                <xs:enumeration value="Δευτέρα"/>
                <xs:enumeration value="Τρίτη"/>
                <xs:enumeration value="Τετάρτη"/>
                <xs:enumeration value="Πέμπτη"/>
                <xs:enumeration value="Παρασκευή"/>
                <xs:enumeration value="Σάββατο"/>
                <xs:enumeration value="Κυριακή"/>
            </xs:restriction>
        </xs:simpleType>

        <!-- Datetime range -->
        <xs:complexType name="datetime_range_type">
            <xs:sequence>
                <xs:element name="day" type="tns:day_type"/>
                <xs:element name="from" type="xs:time"/>
                <xs:element name="to" type="xs:time"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Datetime range -->
        <xs:complexType name="array_of_datetime_ranges_type">
            <xs:sequence>
                <xs:element name="range" type="tns:datetime_range_type" minOccurs="0" maxOccurs="1"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Core person data -->
        <xs:complexType name="person_type">
            <xs:sequence>
                <xs:element name="first_name" type="xs:string"/>
                <xs:element name="last_name" type="xs:string"/>
                <xs:element name="email" type="tns:email_type"/>
                <xs:element name="is_admin" type="xs:boolean"/>
            </xs:sequence>
            <xs:attribute name="id" type="xs:ID" use="optional"/>
        </xs:complexType>

        <!-- Business -->
        <xs:complexType name="business_type">
            <xs:sequence>
                <xs:element name="username" type="xs:string"/>
                <xs:element name="password" type="xs:string"/>
                <xs:element name="name" type="xs:string"/>
                <xs:element name="afm" type="tns:afm_type"/>
                <xs:element name="email" type="tns:email_type"/>
                <xs:element name="phone" type="xs:string"/>
                <xs:element name="address" type="tns:address_type"/>
                <xs:element name="photos" type="tns:array_of_photos_type" minOccurs="0"/>
                <xs:element name="opening_hours" type="tns:array_of_datetime_ranges_type"/>
            </xs:sequence>
            <xs:attribute name="id" type="xs:ID" use="optional"/>
        </xs:complexType>

        <!-- An enevelope of multiple users -->
        <xs:complexType name="array_of_users_type">
            <xs:sequence>
                <xs:element name="user" type="tns:person_type" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>

        <!-- An enevelope of multiple businesses -->
        <xs:complexType name="array_of_businesses_type">
            <xs:sequence>
                <xs:element name="business" type="tns:business_type" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>

        <!-- A single up/down vote -->
        <xs:complexType name="vote_type">
            <xs:sequence>
                <xs:element name="user_id" type="xs:IDREF"/>
                <xs:element name="is_upvote" type="xs:boolean"/>
            </xs:sequence>
        </xs:complexType>

        <!-- An envelope for multiple votes -->
        <xs:complexType name="array_of_votes_type">
            <xs:sequence>
                <xs:element name="vote" type="tns:vote_type" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Simple offer -->
        <xs:complexType name="offer_type">
            <xs:sequence>
                <xs:element name="title" type="xs:string"/>
                <xs:element name="description" type="xs:string"/>
                <xs:element name="date_begin" type="xs:dateTime"/>
                <xs:element name="date_end" type="xs:dateTime"/>
                <xs:element name="is_published" type="xs:boolean"/>
                <xs:element name="photos" type="tns:array_of_photos_type" minOccurs="0"/>
                <xs:element name="votes" type="tns:array_of_votes_type">
                    <xs:unique name="one_vote_per_person">
                        <xs:selector xpath="*/vote"></xs:selector>
                        <xs:field xpath="user_id"></xs:field>
                    </xs:unique>
                </xs:element>
            </xs:sequence>
            <xs:attribute name="id" type="xs:ID" use="optional"/>
        </xs:complexType>

        <!-- Repeat offer -->
        <xs:complexType name="repeat_offer_type">
            <xs:complexContent>
                <xs:extension base="tns:offer_type">
                    <xs:sequence>
                        <xs:element name="offer_range" type="tns:array_of_datetime_ranges_type"/>
                    </xs:sequence>
                </xs:extension>
            </xs:complexContent>
        </xs:complexType>

        <!-- Reservation info for a single coupon -->
        <xs:complexType name="coupon_reservation_type">
            <xs:sequence>
                <xs:element name="user_id" type="xs:IDREF"/>
                <xs:element name="expiration_date" type="xs:dateTime" minOccurs="0"/>
            </xs:sequence>
        </xs:complexType>

        <!-- An envelope for multiple coupon reservations -->
        <xs:complexType name="array_of_coupon_reservations_type">
            <xs:sequence>
                <xs:element name="reservation" type="tns:coupon_reservation_type" minOccurs="0" maxOccurs="1"/>
            </xs:sequence>
        </xs:complexType>

        <!-- Limited offer -->
        <xs:complexType name="limited_offer_type">
            <xs:complexContent>
                <xs:extension base="tns:offer_type">
                    <xs:sequence>
                        <xs:element name="max_coupons" type="xs:integer"/>
                        <xs:element name="coupon_expiration_minutes" type="xs:integer" minOccurs="0"/>
                        <xs:element name="coupon_reservations" type="tns:array_of_coupon_reservations_type"/>
                    </xs:sequence>
                </xs:extension>
            </xs:complexContent>
        </xs:complexType>
    </xs:schema>


SQL
---

opendeals.sql ::

    SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
    SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
    SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL';

    DROP SCHEMA IF EXISTS `opendeals` ;
    CREATE SCHEMA IF NOT EXISTS `opendeals` DEFAULT CHARACTER SET utf8 ;
    USE `opendeals` ;

    -- -----------------------------------------------------
    -- Table `opendeals`.`counties`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`counties` ;
    CREATE  TABLE IF NOT EXISTS `opendeals`.`counties` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      PRIMARY KEY (`id`) )
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`municipalities`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`municipalities` ;
    CREATE  TABLE IF NOT EXISTS `opendeals`.`municipalities` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      `county_id` INT NOT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_municipalities_counties` (`county_id` ASC) ,
      CONSTRAINT `fk_municipalities_counties`
        FOREIGN KEY (`county_id`)
        REFERENCES `opendeals`.`counties` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`offer_categories`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`offer_categories` ;
    CREATE  TABLE IF NOT EXISTS `opendeals`.`offer_categories` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      PRIMARY KEY (`id`) )
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`days`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`days` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`days` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      PRIMARY KEY (`id`) )
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`users`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`users` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`users` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `username` MEDIUMTEXT NOT NULL ,
      `password` MEDIUMTEXT NOT NULL ,
      `email` MEDIUMTEXT NOT NULL ,
      `token` MEDIUMTEXT NULL DEFAULT NULL ,
      `email_verified` TINYINT(1) NOT NULL DEFAULT FALSE ,
      `is_banned` TINYINT(1) NOT NULL DEFAULT FALSE ,
      `role` MEDIUMTEXT NOT NULL ,
      `terms_accepted` TINYINT(1) NOT NULL DEFAULT FALSE,
      `last_login` DATETIME NULL DEFAULT NULL,
      PRIMARY KEY (`id`) )
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`companies`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`companies` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`companies` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      `address` MEDIUMTEXT NULL DEFAULT NULL ,
      `postalcode` VARCHAR(5) NULL DEFAULT NULL ,
      `phone` VARCHAR(10) NOT NULL ,
      `fax` VARCHAR(10) NULL DEFAULT NULL ,
      `service_type` MEDIUMTEXT NULL DEFAULT NULL ,
      `afm` VARCHAR(9) NOT NULL ,
      `latitude` DOUBLE NULL DEFAULT NULL ,
      `longitude` DOUBLE NULL DEFAULT NULL ,
      `is_enabled` TINYINT(1) NOT NULL DEFAULT FALSE ,
      `user_id` INT NOT NULL ,
      `municipality_id` INT NULL DEFAULT NULL ,
      `image_count` INT NOT NULL DEFAULT 0 ,
      `work_hour_count` INT NOT NULL DEFAULT 0,
      `created` DATETIME NULL DEFAULT NULL ,
      `modified` DATETIME NULL DEFAULT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_companies_users1` (`user_id` ASC) ,
      INDEX `fk_companies_municipalities` (`municipality_id` ASC) ,
      CONSTRAINT `fk_companies_users1`
        FOREIGN KEY (`user_id` )
        REFERENCES `opendeals`.`users` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_companies_municipalities`
        FOREIGN KEY (`municipality_id` )
        REFERENCES `opendeals`.`municipalities` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`images`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`images` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`images` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `name` MEDIUMTEXT NOT NULL ,
      `type` MEDIUMTEXT NOT NULL ,
      `size` INT NOT NULL DEFAULT 0,
      `size_thumb` INT NOT NULL DEFAULT 0,
      `error`INT NULL DEFAULT NULL ,
      `data` LONGBLOB NOT NULL ,
      `data_thumb` LONGBLOB NOT NULL ,
      `offer_id` INT NULL DEFAULT NULL ,
      `company_id` INT NULL DEFAULT NULL ,
      `image_category` INT NOT NULL ,
      `created` DATETIME NULL DEFAULT NULL ,
      `modified` DATETIME NULL DEFAULT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_images_offers` (`offer_id` ASC) ,
      INDEX `fk_images_companies` (`company_id` ASC) ,
      CONSTRAINT `fk_images_offers`
        FOREIGN KEY (`offer_id`)
        REFERENCES `opendeals`.`offers` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_images_companies`
        FOREIGN KEY (`company_id`)
        REFERENCES `opendeals`.`companies` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`offers`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`offers` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`offers` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `title` TEXT NOT NULL ,
      `description` TEXT NULL DEFAULT NULL ,
      `started` DATETIME NULL DEFAULT NULL ,
      `ended` DATETIME NULL DEFAULT NULL ,
      `autostart` DATETIME NULL DEFAULT NULL ,
      `autoend` DATETIME NULL DEFAULT NULL ,
      `coupon_terms` TEXT NULL DEFAULT NULL ,
      `total_quantity` INT NOT NULL DEFAULT 0 ,
      `coupon_count` INT NOT NULL DEFAULT 0 ,
      `max_per_student` int(11) NOT NULL DEFAULT 0,
      `tags` MEDIUMTEXT NULL ,
      `offer_category_id` INT NOT NULL ,
      `offer_type_id` INT NOT NULL ,
      `company_id` INT NOT NULL ,
      `image_count` INT NOT NULL DEFAULT 0 ,
      `work_hour_count` INT NOT NULL DEFAULT 0 ,
      `offer_state_id` INT NOT NULL DEFAULT 1 ,
      `is_spam` TINYINT(1) NOT NULL DEFAULT 0 ,
      `vote_count` INT(11) NOT NULL DEFAULT 0 ,
      `vote_plus` INT(11) NOT NULL DEFAULT 0 ,
      `vote_minus` INT(11) NOT NULL DEFAULT 0 ,
      `created` DATETIME NULL DEFAULT NULL ,
      `modified` DATETIME NULL DEFAULT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_offers_offer_categories` (`offer_category_id` ASC) ,
      INDEX `fk_offers_offer_types1` (`offer_type_id` ASC) ,
      INDEX `fk_offers_companies1` (`company_id` ASC) ,
      INDEX `fk_offers_offer_states` (`offer_state_id` ASC) ,
      CONSTRAINT `fk_offers_offer_categories`
        FOREIGN KEY (`offer_category_id` )
        REFERENCES `opendeals`.`offer_categories` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_offers_companies1`
        FOREIGN KEY (`company_id` )
        REFERENCES `opendeals`.`companies` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`students`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`students` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`students` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `firstname` MEDIUMTEXT NOT NULL ,
      `lastname` MEDIUMTEXT NOT NULL ,
      `receive_email` TINYINT(1) NOT NULL DEFAULT FALSE ,
      `user_id` INT NOT NULL ,
      `image_id` INT NULL DEFAULT NULL ,
      `created` DATETIME NULL DEFAULT NULL ,
      `modified` DATETIME NULL DEFAULT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_students_users1` (`user_id` ASC) ,
      INDEX `fk_students_images` (`image_id` ASC) ,
      CONSTRAINT `fk_students_users1`
        FOREIGN KEY (`user_id` )
        REFERENCES `opendeals`.`users` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_students_images`
        FOREIGN KEY (`image_id` )
        REFERENCES `opendeals`.`images` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`coupons`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`coupons` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`coupons` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `serial_number` TEXT NOT NULL ,
      `created` DATETIME NULL DEFAULT NULL ,
      `modified` DATETIME NULL DEFAULT NULL ,
      `is_used` TINYINT(1)  NOT NULL DEFAULT 0 ,
      `offer_id` INT NOT NULL ,
      `student_id` INT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_coupons_offers1` (`offer_id` ASC) ,
      INDEX `fk_coupons_students1` (`student_id` ASC) ,
      CONSTRAINT `fk_coupons_offers1`
        FOREIGN KEY (`offer_id` )
        REFERENCES `opendeals`.`offers` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_coupons_students1`
        FOREIGN KEY (`student_id` )
        REFERENCES `opendeals`.`students` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`work_hours`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`work_hours` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`work_hours` (
      `id` INT NOT NULL AUTO_INCREMENT ,
      `day_id` INT NOT NULL ,
      `starting` TIME NOT NULL ,
      `ending` TIME NOT NULL ,
      `company_id` INT NULL ,
      `offer_id` INT NULL ,
      PRIMARY KEY (`id`) ,
      INDEX `fk_work_hours_days1` (`day_id` ASC) ,
      INDEX `fk_work_hours_companies1` (`company_id` ASC) ,
      INDEX `fk_work_hours_offers1` (`offer_id` ASC) ,
      CONSTRAINT `fk_work_hours_days1`
        FOREIGN KEY (`day_id` )
        REFERENCES `opendeals`.`days` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_work_hours_companies1`
        FOREIGN KEY (`company_id` )
        REFERENCES `opendeals`.`companies` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_work_hours_offers1`
        FOREIGN KEY (`offer_id` )
        REFERENCES `opendeals`.`offers` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`votes`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`votes` ;

    CREATE  TABLE IF NOT EXISTS `opendeals`.`votes` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `offer_id` int(11) NOT NULL,
      `student_id` int(11) NOT NULL,
      `vote` tinyint(1) NOT NULL COMMENT '0 negative, 1 positive',
      PRIMARY KEY (`id`) ,
      INDEX `fk_votes_offers1` (`offer_id` ASC) ,
      INDEX `fk_votes_students1` (`student_id` ASC) ,
      CONSTRAINT `fk_votes_offers1`
        FOREIGN KEY (`offer_id` )
        REFERENCES `opendeals`.`offers` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_votes_students1`
        FOREIGN KEY (`student_id` )
        REFERENCES `opendeals`.`students` (`id` )
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    -- -----------------------------------------------------
    -- Table `opendeals`.`distances`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `opendeals`.`distances`;

    CREATE TABLE IF NOT EXISTS `opendeals`.`distances` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `user_id` int(11) NOT NULL,
      `company_id` int(11) NOT NULL,
      `radius` int(11) NOT NULL,
      `distance` double NOT NULL,
      PRIMARY KEY (`id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;


    DELIMITER //
    CREATE FUNCTION `opendeals`.`geodist` (fromlat DOUBLE, fromlng DOUBLE, tolat DOUBLE, tolng DOUBLE)
    RETURNS DOUBLE
    DETERMINISTIC
    BEGIN
    DECLARE latfrom,latto,latdiff,lngdiff DOUBLE;
    DECLARE lathvr,lnghvr,root,dist DOUBLE;
    DECLARE r INT;
    SET r = 6371;
    SET latfrom = radians(fromlat);
    SET latto = radians(tolat);
    SET latdiff = radians(tolat - fromlat);
    SET lngdiff = radians(tolng - fromlng);
    SET lathvr = sin(latdiff / 2) * sin(latdiff / 2);
    SET lnghvr = sin(lngdiff / 2) * sin(lngdiff / 2);
    SET root = sqrt(lathvr + cos(latfrom) * cos(latto) * sin(lnghvr));
    SET dist = 2 * r * asin(root);
    RETURN dist;
    END //


    DELIMITER //
    CREATE PROCEDURE `opendeals`.`updatedistances` (IN uid INT, IN lat DOUBLE, IN lng DOUBLE, IN r INT)
    BEGIN
    DELETE FROM `opendeals`.`distances` WHERE `distances`.`user_id` = uid;
    INSERT INTO `opendeals`.`distances` (user_id, company_id, radius, distance)
    SELECT users.id, companies.id, r, geodist(lat,lng,companies.latitude,companies.longitude) AS d
    FROM users,companies
    WHERE users.id = uid
    GROUP BY companies.id
    HAVING d > 0 AND d < r
    ORDER BY d ASC;
    END //


    SET SQL_MODE=@OLD_SQL_MODE;
    SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
    SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;


insert_counties.sql ::

    LOAD DATA LOCAL INFILE "counties.csv"
    INTO TABLE `opendeals`.`counties`
    CHARACTER SET 'UTF8'
    FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    IGNORE 1 LINES
    (id, name);


insert_days.sql ::

    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Δευτέρα');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Τρίτη');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Τετάρτη');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Πέμπτη');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Παρασκευή');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Σάββατο');
    INSERT INTO `days` (`id`,`name`) VALUES (NULL,'Κυριακή');


insert_municipalities.sql ::

    LOAD DATA LOCAL INFILE "municipalities.csv"
    INTO TABLE `opendeals`.`municipalities`
    CHARACTER SET 'UTF8'
    FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    IGNORE 1 LINES
    (id, county_id, name);


insert_offer_categores.sql ::

    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Φαγητό');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Υπηρεσίες');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Δραστηριότητες & Χόμπι');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Ένδυση & Υπόδηση');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Υγεία');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Ταξίδια & Εκδρομές');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Διασκέδαση');
    INSERT INTO `offer_categories` (`id`,`name`) VALUES (NULL,'Προϊόντα');


counties.csv ::

    id,name
    1,Δράμας
    2,Έβρου
    3,Καβάλας
    4,Ξάνθης
    5,Ροδόπης
    6,Ημαθίας
    7,Θεσσαλονίκης
    8,Κιλκίς
    9,Πέλλης
    10,Πιερίας
    11,Σερρών
    12,Χαλκιδικής
    13,Γρεβενών
    14,Καστοριάς
    15,Κοζάνης
    16,Φλωρίνης
    17,Άρτης
    18,Θεσπρωτίας
    19,Ιωαννίνων
    20,Πρεβέζης
    21,Καρδίτσης
    22,Λαρίσης
    23,Μαγνησίας
    24,Τρικάλων
    25,Ζακύνθου
    26,Κερκύρας
    27,Κεφαλληνίας
    28,Λευκάδος
    29,Αιτωλοακαρνανίας
    30,Αχαϊας
    31,Ηλείας
    32,Βοιωτίας
    33,Ευβοίας
    34,Ευρυτανίας
    35,Φθιώτιδος
    36,Φωκίδος
    37,Αττικής-Βορείου Τομέα Αθηνών
    38,Αττικής-Δυτικού Τομέα Αθηνών
    39,Αττικής-Κεντρικού Τομέα Αθηνών
    40,Αττικής-Νοτίου Τομέα Αθηνών
    41,Αττικής-Ανατολικής Αττικής
    42,Αττικής-Δυτικής Αττικής
    43,Αττικής-Πειραιώς
    44,Αττικής-Νήσων
    45,Αργολίδος
    46,Αρκαδίας
    47,Κορινθίας
    48,Λακωνίας
    49,Μεσσηνίας
    50,Λέσβου
    51,Σάμου
    52,Χίου
    53,Κυκλάδων
    54,Δωδεκανήσου
    55,Ηρακλείου
    56,Λασιθίου
    57,Ρεθύμνης
    58,Χανίων

municipalities.csv ::

    id,county_id,name
    1,1,Δοξάτου
    2,1,Δράμας
    3,1,Κάτω Νευροκοπίου
    4,1,Παρανεστίου
    5,1,Προσοτσάνης
    6,2,Αλεξανδρούπολης
    7,2,Διδυμοτείχου
    8,2,Ορεστιάδας
    9,2,Σαμοθράκης
    10,2,Σουφλίου
    11,3,Θάσου
    12,3,Καβάλας
    13,3,Νέστου
    14,3,Παγγαίου
    15,4,Αβδήρων
    16,4,Μύκης
    17,4,Ξάνθης
    18,4,Τοπείρου
    19,5,Αρριανών
    20,5,Ιάσμου
    21,5,Κομοτηνής
    22,5,Μαρωνείας - Σαπών
    23,6,Αλεξάνδρειας
    24,6,Βέροιας
    25,6,Νάουσας
    26,7,Αμπελοκήπων - Μενεμένης
    27,7,Βόλβης
    28,7,Δέλτα
    29,7,Θερμαϊκού
    30,7,Θέρμης
    31,7,Θεσσαλονίκης
    32,7,Καλαμαριάς
    33,7,Κορδελιού - Ευόσμου
    34,7,Λαγκαδά
    35,7,Νέαπολης - Συκεών
    36,7,Παύλου Μελά
    37,7,Πυλαίας - Χορτιάτη
    38,7,Χαλκηδόνος
    39,7,Ωραιοκάστρου
    40,8,Κιλκίς
    41,8,Παιονίας
    42,9,Αλμωπίας
    43,9,Έδεσσας
    44,9,Πέλλας
    45,9,Σκύδρας
    46,10,Δίου - Ολύμπου
    47,10,Κατερίνης
    48,10,Πύδνας - Κολινδρού
    49,11,Αμφίπολης
    50,11,Βισαλτίας
    51,11,Εμμανουήλ Παππά
    52,11,Ηρακλείας
    53,11,Νέας Ζίχνης
    54,11,Σερρών
    55,11,Σιντικής
    56,12,Αριστοτέλη
    57,12,Κασσάνδρας
    58,12,Νέας Προποντίδας
    59,12,Πολυγύρου
    60,12,Σιθωνίας
    61,13,Γρεβενών
    62,13,Δεσκάτης
    63,14,Καστοριάς
    64,14,Νεστορίου
    65,14,Ορεστίδος
    66,15,Βοίου
    67,15,Εορδαίας
    68,15,Κοζάνης
    69,15,Σερβίων - Βελβεντού
    70,16,Αμυνταίου
    71,16,Πρεσπών
    72,16,Φλώρινας
    73,17,Αρταίων
    74,17,Γεωργίου Καραϊσκάκη
    75,17,Κεντρικών Τζουμέρκων
    76,17,Νικολάου Σκουφά
    77,18,Ηγουμενίτσας
    78,18,Σουλίου
    79,18,Φιλιατών
    80,19,Βορείων Τζουμέρκων
    81,19,Δωδώνης
    82,19,Ζαγορίου
    83,19,Ζίτσας
    84,19,Ιωαννιτών
    85,19,Κόνιτσας
    86,19,Μετσόβου
    87,19,Πωγωνίου
    88,20,Ζηρού
    89,20,Πάργας
    90,20,Πρέβεζας
    91,21,Αργιθέας
    92,21,Καρδίτσας
    93,21,Λίμνης Πλαστήρα
    94,21,Μουζακίου
    95,21,Παλαμά
    96,21,Σοφάδων
    97,22,Αγιάς
    98,22,Ελασσόνας
    99,22,Κιλελέρ
    100,22,Λαρισαίων
    101,22,Δ.Τεμπών
    102,22,Τυρνάβου
    103,22,Φαρσάλων
    104,23,Αλμυρού
    105,23,Βόλου
    106,23,Ζαγοράς - Μουρεσίου
    107,23,Νοτίου Πηλίου
    108,23,Ρήγα Φερραίου
    109,23,Αλοννήσου
    110,23,Σκιάθου
    111,23,Σκοπέλου
    112,24,Καλαμπάκας
    113,24,Πύλης
    114,24,Τρικκαίων
    115,24,Φαρκαδόνας
    116,25,Ζακύνθου
    117,26,Κέρκυρας
    118,26,Παξών
    119,27,Κεφαλονιάς
    120,27,Ιθάκης
    121,28,Λευκάδος
    122,28,Μεγανησίου
    123,29,Αγρινίου
    124,29,Άκτιου - Βόνιτσας
    125,29,Αμφιλοχίας
    126,29,Θέρμου
    127,29,Ιεράς Πόλης Μεσολογγίου
    128,29,Ναυπακτίας
    129,29,Ξηρομέρου
    130,30,Αιγιαλείας
    131,30,Δυτικής Αχαΐας
    132,30,Ερυμάνθου
    133,30,Καλαβρύτων
    134,30,Πατρέων
    135,31,Ανδραβίδας - Κυλλήνης
    136,31,Ανδρίτσαινας - Κρεστένων
    137,31,Αρχαίας Ολυμπίας
    138,31,Ζαχάρως
    139,31,Ήλιδας
    140,31,Πηνειού
    141,31,Πύργου
    142,32,Αλιάρτου
    143,32,Διστόμου-Αράχοβας - Αντίκυρας
    144,32,Θηβαίων
    145,32,Λεβαδέων
    146,32,Ορχομενού
    147,32,Τανάγρας
    148,33,Διρφύων - Μεσσαπίων
    149,33,Ερέτριας
    150,33,Ιστιαίας - Αιδηψού
    151,33,Καρύστου
    152,33,Κύμης - Αλιβερίου
    153,33,Μαντουδίου - Λίμνης - Αγίας Άννας
    154,33,Σκύρου
    155,33,Χαλκιδέων
    156,34,Καρπενησίου
    157,35,Μώλου - Αγίου Κωνσταντίνου
    158,35,Αμφίκλειας - Ελάτειας
    159,35,Λοκρών
    160,35,Δομοκού
    161,35,Λαμιέων
    162,35,Μακρακώμης
    163,35,Στυλίδος
    164,36,Δελφών
    165,36,Δωρίδος
    166,37,Αγίας Παρασκευής
    167,37,Αμαρουσίου
    168,37,Βριλησσίων
    169,37,Ηρακλείου
    170,37,Κηφισιάς
    171,37,Λυκόβρυσης - Πεύκης
    172,37,Μεταμορφώσεως
    173,37,Νέας Ιωνίας
    174,37,Παπάγου - Χαλαργού
    175,37,Πεντέλης
    176,37,Φιλοθέης - Ψυχικού
    177,37,Χαλανδρίου
    178,38,Αγίας Βαρβάρας
    179,38,Αγίων Αναργύρων - Καματερού
    180,38,Αιγάλεω
    181,38,Ιλίου
    182,38,Περιστερίου
    183,38,Πετρούπολης
    184,38,Χαϊδαρίου
    185,39,Αθηναίων
    186,39,Βύρωνος
    187,39,Γαλατσίου
    188,39,Δάφνης - Υμηττού
    189,39,Ζωγράφου
    190,39,Ηλιουπόλεως
    191,39,Καισαριανής
    192,39,Φιλαδέλφειας - Χαλκηδόνος
    193,40,Αγίου Δημητρίου
    194,40,Αλίμου
    195,40,Γλυφάδας
    196,40,Ελληνικού - Αργυρούπολης
    197,40,Καλλιθέας
    198,40,Μοσχάτου - Ταύρου
    199,40,Νέας Σμύρνης
    200,40,Παλαιού Φαλήρου
    201,41,Αχαρνών
    202,41,Βάρης - Βούλας - Βουλιαγμένης
    203,41,Διονύσου
    204,41,Κρωπίας
    205,41,Λαυρεωτικής
    206,41,Μαραθώνος
    207,41,Μαρκοπούλου Μεσογαίας
    208,41,Παιανίας
    209,41,Παλλήνης
    210,41,Ραφήνας - Πικερμίου
    211,41,Σαρωνικού
    212,41,Σπάτων - Αρτέμιδος
    213,41,Ωρωπίων
    214,42,Ασπροπύργου
    215,42,Ελευσίνας
    216,42,Μάνδρας - Ειδυλλίας
    217,42,Μεγαρέων
    218,42,Φυλής
    219,43,Κερατσινίου - Δραπετσώνας
    220,43,Κορυδαλλού
    221,43,Νικαίας - Αγίου Ιωάννου Ρέντη
    222,43,Πειραιώς
    223,43,Περάματος
    224,44,Αγκιστρίου
    225,44,Αίγινας
    226,44,Κυθήρων
    227,44,Πόρου
    228,44,Σαλαμίνας
    229,44,Σπετσών
    230,44,Τροιζηνίας
    231,44,Ύδρας
    232,45,Άργους - Μυκηνών
    233,45,Επιδαύρου
    234,45,Ερμιονόδας
    235,45,Ναυπλιέων
    236,46,Βόρειας Κυνουρίας
    237,46,Γόρτυνος
    238,46,Μεγαλόπολης
    239,46,Νότιας Κυνουρίας
    240,46,Τρίπολης
    241,47,Βέλου - Βόχας
    242,47,Κορινθίων
    243,47,Λουτρακίου - Αγ. Θεοδώρων
    244,47,Νεμέας
    245,47,Ξυλοκάστρου - Ευρωστίνης
    246,47,Σικυωνίων
    247,48,Ανατολικής Μάνης
    248,48,Ελαφονήσου
    249,48,Ευρώτα
    250,48,Μονεμβασιάς
    251,48,Σπάρτης
    252,49,Δυτικής Μάνης
    253,49,Καλαμάτας
    254,49,Μεσσήνης
    255,49,Οιχαλίας
    256,49,Πύλου - Νέστορος
    257,49,Τριφυλίας
    258,50,Λέσβου
    259,50,Αγίου Ευστρατίου
    260,50,Λήμνου
    261,51,Σάμου
    262,51,Ικαρίας
    263,51,Φούρνων Κορσέων
    264,52,Οινουσσών
    265,52,Χίου
    266,52,Ψαρών
    267,53,Άνδρου
    268,53,Ανάφης
    269,53,Θήρας
    270,53,Ιητών
    271,53,Σικίνου
    272,53,Φολέγανδρου
    273,53,Κέας
    274,53,Κύθνου
    275,53,Κιμώλου
    276,53,Μήλου
    277,53,Σερίφου
    278,53,Σίφνου
    279,53,Μυκόνου
    280,53,Αμοργού
    281,53,Νάξου  Μικρών Κυκλάδων
    282,53,Αντιπάρου
    283,53,Πάρου
    284,53,Σύρου - Ερμούπολης
    285,53,Τήνου
    286,54,Αγαθονησίου
    287,54,Αστυπάλαιας
    288,54,Καλυμνίων
    289,54,Καρπάθου
    290,54,Κάσου
    291,54,Λειψών
    292,54,Λέρου
    293,54,Πάτμου
    294,54,Κώ
    295,54,Νισύρου
    296,54,Μεγίστης
    297,54,Ρόδου
    298,54,Σύμης
    299,54,Τήλου
    300,54,Χάλκης
    301,55,Αρχανών - Αστερουσίων
    302,55,Βιάννου
    303,55,Γόρτυνας
    304,55,Ηρακλείου
    305,55,Μαλεβιζίου
    306,55,Μινώα Πεδιάδας
    307,55,Φαιστού
    308,55,Χερσονήσου
    309,56,Αγίου Νικολάου
    310,56,Ιεράπετρας
    311,56,Οροπεδίου Λασιθίου
    312,56,Σητείας
    313,57,Αγίου Βασιλείου
    314,57,Αμάριου
    315,57,Ανωγείων
    316,57,Μυλοποτάμου
    317,57,Ρεθύμνης
    318,58,Αποκορώνου
    319,58,Γαύδου
    320,58,Καντάνου - Σέλινου
    321,58,Κισσάμου
    322,58,Πλατανιά
    323,58,Σφακίων
    324,58,Χανίων

