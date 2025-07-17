# Curso 4: Bases de Datos Maestría - SQL y NoSQL - Completo

## 📋 Información General
- **Título**: "Database Master: TaskDB Pro - SQL, NoSQL y Data Architecture"
- **Duración**: 10 horas (40 videos de 15 min c/u)
- **Nivel**: Intermedio-Avanzado (requiere Cursos 1, 2 y 3)
- **Proyecto Principal**: TaskDB Pro - Sistema de Base de Datos Híbrido
- **Mini-proyectos**: 40 proyectos únicos (1 por video)
- **Precio Sugerido**: $149.99

## 🐳 Docker Setup para Multi-Database Stack

### docker-compose.yml para Full Database Stack:
```yaml
version: '3.8'
services:
  # PostgreSQL - SQL Database
  postgres-primary:
    image: postgres:15-alpine
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DB=taskdb_pro
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password123
      - POSTGRES_REPLICATION_MODE=master
      - POSTGRES_REPLICATION_USER=replica_user
      - POSTGRES_REPLICATION_PASSWORD=replica_pass
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./docker/postgres-init.sql:/docker-entrypoint-initdb.d/init.sql
      - ./docker/postgresql.conf:/etc/postgresql/postgresql.conf
    command: postgres -c config_file=/etc/postgresql/postgresql.conf
    restart: unless-stopped

  # PostgreSQL Read Replica
  postgres-replica:
    image: postgres:15-alpine
    ports:
      - "5433:5432"
    environment:
      - POSTGRES_DB=taskdb_pro
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password123
      - POSTGRES_MASTER_SERVICE=postgres-primary
    volumes:
      - postgres_replica_data:/var/lib/postgresql/data
    depends_on:
      - postgres-primary
    restart: unless-stopped

  # MongoDB Replica Set - Primary
  mongodb-primary:
    image: mongo:6.0
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password123
    volumes:
      - mongodb_primary_data:/data/db
      - ./docker/mongo-replica-init.js:/docker-entrypoint-initdb.d/replica-init.js:ro
    command: mongod --replSet rs0 --bind_ip_all
    restart: unless-stopped

  # MongoDB Replica Set - Secondary 1
  mongodb-secondary1:
    image: mongo:6.0
    ports:
      - "27018:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password123
    volumes:
      - mongodb_secondary1_data:/data/db
    command: mongod --replSet rs0 --bind_ip_all
    depends_on:
      - mongodb-primary
    restart: unless-stopped

  # MongoDB Replica Set - Secondary 2 (Arbiter)
  mongodb-arbiter:
    image: mongo:6.0
    ports:
      - "27019:27017"
    command: mongod --replSet rs0 --bind_ip_all --port 27017
    depends_on:
      - mongodb-primary
    restart: unless-stopped

  # Redis Cluster Node 1
  redis-node1:
    image: redis:7-alpine
    ports:
      - "7001:7001"
    command: redis-server --port 7001 --cluster-enabled yes --cluster-config-file nodes-7001.conf --cluster-node-timeout 5000 --appendonly yes --appendfilename appendonly-7001.aof
    volumes:
      - redis_node1_data:/data
    restart: unless-stopped

  # Redis Cluster Node 2
  redis-node2:
    image: redis:7-alpine
    ports:
      - "7002:7002"
    command: redis-server --port 7002 --cluster-enabled yes --cluster-config-file nodes-7002.conf --cluster-node-timeout 5000 --appendonly yes --appendfilename appendonly-7002.aof
    volumes:
      - redis_node2_data:/data
    restart: unless-stopped

  # Redis Cluster Node 3
  redis-node3:
    image: redis:7-alpine
    ports:
      - "7003:7003"
    command: redis-server --port 7003 --cluster-enabled yes --cluster-config-file nodes-7003.conf --cluster-node-timeout 5000 --appendonly yes --appendfilename appendonly-7003.aof
    volumes:
      - redis_node3_data:/data
    restart: unless-stopped

  # ClickHouse - Analytics Database
  clickhouse:
    image: clickhouse/clickhouse-server:23.3-alpine
    ports:
      - "8123:8123"
      - "9000:9000"
    environment:
      - CLICKHOUSE_DB=taskdb_analytics
      - CLICKHOUSE_USER=default
      - CLICKHOUSE_PASSWORD=password123
    volumes:
      - clickhouse_data:/var/lib/clickhouse
      - ./docker/clickhouse-init.sql:/docker-entrypoint-initdb.d/init.sql
    restart: unless-stopped

  # Neo4j - Graph Database
  neo4j:
    image: neo4j:5.8-community
    ports:
      - "7474:7474"
      - "7687:7687"
    environment:
      - NEO4J_AUTH=neo4j/password123
      - NEO4J_PLUGINS=["apoc", "graph-data-science"]
      - NEO4J_dbms_security_procedures_unrestricted=apoc.*,gds.*
    volumes:
      - neo4j_data:/data
      - neo4j_logs:/logs
      - neo4j_import:/var/lib/neo4j/import
      - neo4j_plugins:/plugins
    restart: unless-stopped

  # Apache Cassandra - Wide Column Store
  cassandra:
    image: cassandra:4.1
    ports:
      - "9042:9042"
    environment:
      - CASSANDRA_CLUSTER_NAME=TaskCluster
      - CASSANDRA_DC=datacenter1
      - CASSANDRA_RACK=rack1
      - CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch
    volumes:
      - cassandra_data:/var/lib/cassandra
    restart: unless-stopped

  # ScyllaDB - High Performance Cassandra Alternative
  scylladb:
    image: scylladb/scylla:5.1
    ports:
      - "9043:9042"
    command: --seeds=scylladb --smp 1 --memory 750M --overprovisioned 1 --api-address 0.0.0.0
    volumes:
      - scylladb_data:/var/lib/scylla
    restart: unless-stopped

  # MinIO - S3 Compatible Object Storage
  minio:
    image: minio/minio:latest
    ports:
      - "9001:9000"
      - "9002:9001"
    environment:
      - MINIO_ROOT_USER=minioadmin
      - MINIO_ROOT_PASSWORD=minioadmin123
    volumes:
      - minio_data:/data
    command: server /data --console-address ":9001"
    restart: unless-stopped

  # Database Administration Tools
  adminer:
    image: adminer:latest
    ports:
      - "8080:8080"
    environment:
      - ADMINER_DEFAULT_SERVER=postgres-primary
    restart: unless-stopped

  mongo-express:
    image: mongo-express:latest
    ports:
      - "8081:8081"
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password123
      - ME_CONFIG_MONGODB_URL=mongodb://admin:password123@mongodb-primary:27017/
    depends_on:
      - mongodb-primary
    restart: unless-stopped

  # Database Connection Pooler
  pgbouncer:
    image: pgbouncer/pgbouncer:latest
    ports:
      - "6432:6432"
    environment:
      - DATABASES_HOST=postgres-primary
      - DATABASES_PORT=5432
      - DATABASES_USER=postgres
      - DATABASES_PASSWORD=password123
      - DATABASES_DBNAME=taskdb_pro
    volumes:
      - ./docker/pgbouncer.ini:/etc/pgbouncer/pgbouncer.ini
    depends_on:
      - postgres-primary
    restart: unless-stopped

  # Database Monitoring
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./docker/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'
    restart: unless-stopped

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin123
    volumes:
      - grafana_data:/var/lib/grafana
      - ./docker/grafana-dashboards:/etc/grafana/provisioning/dashboards
      - ./docker/grafana-datasources:/etc/grafana/provisioning/datasources
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  postgres_data:
  postgres_replica_data:
  mongodb_primary_data:
  mongodb_secondary1_data:
  redis_node1_data:
  redis_node2_data:
  redis_node3_data:
  clickhouse_data:
  neo4j_data:
  neo4j_logs:
  neo4j_import:
  neo4j_plugins:
  cassandra_data:
  scylladb_data:
  minio_data:
  prometheus_data:
  grafana_data:

networks:
  default:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
```

## 🎯 Objetivos de Aprendizaje

Al finalizar este curso, el estudiante será capaz de:
- ✅ Diseñar esquemas robustos en SQL y NoSQL
- ✅ Optimizar consultas complejas y índices
- ✅ Implementar replicación y alta disponibilidad
- ✅ Manejar transacciones ACID y eventual consistency
- ✅ Crear data warehouses y analytics pipelines
- ✅ Implementar sharding y particionamiento
- ✅ Trabajar con Graph, Document, Key-Value y Column stores
- ✅ Monitorear performance y troubleshooting
- ✅ Migrar datos entre diferentes bases de datos
- ✅ Implementar backup y disaster recovery

---

## 📚 Estructura Completa del Curso (10 Secciones - 40 Videos)

### **Sección 1: Database Design Fundamentals (4 videos)**

#### Video 1.1: Database Design Principles y ACID Properties (15 min)
**Mini-Proyecto**: TaskDB Schema Design
- **Objetivo**: Fundamentos de diseño de bases de datos
- **Tecnologías**: ER Diagrams, normalization, ACID
- **Resultado**: Esquema normalizado para TaskDB Pro

**Contenido del Video**:
```sql
-- projects/01-taskdb-schema/schema.sql
-- TaskDB Pro - Complete Database Schema Design

-- Enable necessary extensions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pg_stat_statements";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";

-- Create custom types
CREATE TYPE priority_level AS ENUM ('low', 'medium', 'high', 'urgent');
CREATE TYPE task_status AS ENUM ('todo', 'in_progress', 'review', 'done', 'cancelled');
CREATE TYPE user_role AS ENUM ('admin', 'manager', 'developer', 'viewer');
CREATE TYPE notification_type AS ENUM ('task_assigned', 'task_completed', 'deadline_reminder', 'comment_added');

-- Users table with role-based access
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    role user_role DEFAULT 'viewer',
    avatar_url TEXT,
    is_active BOOLEAN DEFAULT true,
    email_verified BOOLEAN DEFAULT false,
    last_login_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Projects/Workspaces
CREATE TABLE projects (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(200) NOT NULL,
    description TEXT,
    color VARCHAR(7) DEFAULT '#3498db', -- Hex color
    is_archived BOOLEAN DEFAULT false,
    owner_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Project members with roles
CREATE TABLE project_members (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role user_role DEFAULT 'viewer',
    joined_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(project_id, user_id)
);

-- Task categories
CREATE TABLE categories (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    color VARCHAR(7) DEFAULT '#95a5a6',
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(project_id, name)
);

-- Main tasks table
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    title VARCHAR(500) NOT NULL,
    description TEXT,
    status task_status DEFAULT 'todo',
    priority priority_level DEFAULT 'medium',
    
    -- Relationships
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    category_id UUID REFERENCES categories(id) ON DELETE SET NULL,
    assignee_id UUID REFERENCES users(id) ON DELETE SET NULL,
    reporter_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    parent_task_id UUID REFERENCES tasks(id) ON DELETE CASCADE,
    
    -- Dates
    due_date TIMESTAMP WITH TIME ZONE,
    started_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,
    
    -- Tracking
    estimated_hours DECIMAL(8,2),
    actual_hours DECIMAL(8,2) DEFAULT 0,
    
    -- Metadata
    position INTEGER DEFAULT 0, -- For ordering within status
    story_points INTEGER, -- For agile estimation
    
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Constraints
    CONSTRAINT positive_estimated_hours CHECK (estimated_hours >= 0),
    CONSTRAINT positive_actual_hours CHECK (actual_hours >= 0),
    CONSTRAINT positive_story_points CHECK (story_points >= 0),
    CONSTRAINT valid_completed_at CHECK (
        (status = 'done' AND completed_at IS NOT NULL) OR 
        (status != 'done' AND completed_at IS NULL)
    )
);

-- Task dependencies (many-to-many)
CREATE TABLE task_dependencies (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    depends_on_task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(task_id, depends_on_task_id),
    CONSTRAINT no_self_dependency CHECK (task_id != depends_on_task_id)
);

-- Task tags (many-to-many)
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(50) NOT NULL,
    color VARCHAR(7) DEFAULT '#e74c3c',
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(project_id, name)
);

CREATE TABLE task_tags (
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (task_id, tag_id)
);

-- Comments and activity
CREATE TABLE comments (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    is_internal BOOLEAN DEFAULT false, -- Internal comments not visible to clients
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- File attachments
CREATE TABLE attachments (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    uploaded_by UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    filename VARCHAR(255) NOT NULL,
    original_filename VARCHAR(255) NOT NULL,
    file_size BIGINT NOT NULL,
    mime_type VARCHAR(100) NOT NULL,
    file_path TEXT NOT NULL,
    download_count INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT positive_file_size CHECK (file_size > 0)
);

-- Time tracking
CREATE TABLE time_entries (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    description TEXT,
    hours DECIMAL(8,2) NOT NULL,
    date DATE NOT NULL DEFAULT CURRENT_DATE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT positive_hours CHECK (hours > 0)
);

-- Notifications
CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type notification_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    task_id UUID REFERENCES tasks(id) ON DELETE CASCADE,
    is_read BOOLEAN DEFAULT false,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Activity log for audit trail
CREATE TABLE activity_log (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    task_id UUID REFERENCES tasks(id) ON DELETE CASCADE,
    project_id UUID REFERENCES projects(id) ON DELETE CASCADE,
    action VARCHAR(50) NOT NULL,
    old_values JSONB,
    new_values JSONB,
    ip_address INET,
    user_agent TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Indexes for performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_active ON users(is_active);

CREATE INDEX idx_tasks_project_id ON tasks(project_id);
CREATE INDEX idx_tasks_assignee_id ON tasks(assignee_id);
CREATE INDEX idx_tasks_status ON tasks(status);
CREATE INDEX idx_tasks_priority ON tasks(priority);
CREATE INDEX idx_tasks_due_date ON tasks(due_date);
CREATE INDEX idx_tasks_created_at ON tasks(created_at);
CREATE INDEX idx_tasks_position ON tasks(project_id, status, position);

-- Composite indexes for common queries
CREATE INDEX idx_tasks_project_status ON tasks(project_id, status);
CREATE INDEX idx_tasks_assignee_status ON tasks(assignee_id, status);
CREATE INDEX idx_tasks_project_assignee ON tasks(project_id, assignee_id);

-- Full-text search indexes
CREATE INDEX idx_tasks_title_search ON tasks USING gin(to_tsvector('english', title));
CREATE INDEX idx_tasks_description_search ON tasks USING gin(to_tsvector('english', description));
CREATE INDEX idx_comments_search ON comments USING gin(to_tsvector('english', content));

-- Partial indexes for better performance
CREATE INDEX idx_active_tasks ON tasks(project_id, status) WHERE status != 'done';
CREATE INDEX idx_overdue_tasks ON tasks(due_date) WHERE due_date < CURRENT_TIMESTAMP AND status != 'done';
CREATE INDEX idx_unread_notifications ON notifications(user_id) WHERE is_read = false;

-- Functions and triggers for automatic updates
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Apply updated_at triggers
CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_projects_updated_at BEFORE UPDATE ON projects
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_tasks_updated_at BEFORE UPDATE ON tasks
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_comments_updated_at BEFORE UPDATE ON comments
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Function to update task actual_hours from time_entries
CREATE OR REPLACE FUNCTION update_task_actual_hours()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE tasks 
    SET actual_hours = (
        SELECT COALESCE(SUM(hours), 0)
        FROM time_entries 
        WHERE task_id = COALESCE(NEW.task_id, OLD.task_id)
    )
    WHERE id = COALESCE(NEW.task_id, OLD.task_id);
    
    RETURN COALESCE(NEW, OLD);
END;
$$ language 'plpgsql';

CREATE TRIGGER update_task_hours_on_time_entry
    AFTER INSERT OR UPDATE OR DELETE ON time_entries
    FOR EACH ROW EXECUTE FUNCTION update_task_actual_hours();

-- Views for common queries
CREATE VIEW task_summary AS
SELECT 
    t.id,
    t.title,
    t.status,
    t.priority,
    t.due_date,
    t.created_at,
    t.updated_at,
    p.name as project_name,
    c.name as category_name,
    u_assignee.username as assignee_username,
    u_reporter.username as reporter_username,
    t.actual_hours,
    t.estimated_hours,
    CASE 
        WHEN t.due_date < CURRENT_TIMESTAMP AND t.status != 'done' THEN true
        ELSE false
    END as is_overdue,
    (
        SELECT COUNT(*)
        FROM comments co
        WHERE co.task_id = t.id
    ) as comment_count,
    (
        SELECT COUNT(*)
        FROM attachments a
        WHERE a.task_id = t.id
    ) as attachment_count
FROM tasks t
JOIN projects p ON t.project_id = p.id
LEFT JOIN categories c ON t.category_id = c.id
LEFT JOIN users u_assignee ON t.assignee_id = u_assignee.id
JOIN users u_reporter ON t.reporter_id = u_reporter.id;

-- Project statistics view
CREATE VIEW project_statistics AS
SELECT 
    p.id,
    p.name,
    COUNT(t.id) as total_tasks,
    COUNT(CASE WHEN t.status = 'done' THEN 1 END) as completed_tasks,
    COUNT(CASE WHEN t.status != 'done' THEN 1 END) as pending_tasks,
    COUNT(CASE WHEN t.due_date < CURRENT_TIMESTAMP AND t.status != 'done' THEN 1 END) as overdue_tasks,
    ROUND(
        (COUNT(CASE WHEN t.status = 'done' THEN 1 END)::decimal / NULLIF(COUNT(t.id), 0)) * 100, 
        2
    ) as completion_percentage,
    COALESCE(SUM(t.actual_hours), 0) as total_hours_logged,
    COALESCE(SUM(t.estimated_hours), 0) as total_estimated_hours
FROM projects p
LEFT JOIN tasks t ON p.id = t.project_id
GROUP BY p.id, p.name;

-- Sample data for testing
INSERT INTO users (email, username, first_name, last_name, role) VALUES
('admin@taskdb.com', 'admin', 'Admin', 'User', 'admin'),
('john@taskdb.com', 'john_dev', 'John', 'Developer', 'developer'),
('sarah@taskdb.com', 'sarah_pm', 'Sarah', 'Manager', 'manager'),
('mike@taskdb.com', 'mike_qa', 'Mike', 'Tester', 'developer');

INSERT INTO projects (name, description, owner_id) VALUES
('TaskDB Pro Development', 'Main development project for TaskDB Pro application', 
 (SELECT id FROM users WHERE username = 'admin')),
('Mobile App', 'Mobile application development project', 
 (SELECT id FROM users WHERE username = 'sarah_pm'));
```

#### Video 1.2: PostgreSQL Advanced Features y Performance (15 min)
**Mini-Proyecto**: Advanced PostgreSQL Setup
- **Objetivo**: Features avanzadas de PostgreSQL
- **Tecnologías**: JSONB, Arrays, CTEs, Window Functions
- **Resultado**: Base de datos PostgreSQL optimizada

#### Video 1.3: MongoDB Document Design Patterns (15 min)
**Mini-Proyecto**: Document Schema Design
- **Objetivo**: Patrones de diseño NoSQL
- **Tecnologías**: MongoDB, document embedding, references
- **Resultado**: Esquema MongoDB optimizado

#### Video 1.4: Database Selection Criteria (15 min)
**Mini-Proyecto**: Database Decision Matrix
- **Objetivo**: Criterios para selección de BD
- **Tecnologías**: CAP theorem, use case analysis
- **Resultado**: Framework de decisión para BD

---

### **Sección 2: SQL Avanzado y Optimización (4 videos)**

#### Video 2.1: Complex Queries y JOINs (15 min)
**Mini-Proyecto**: Advanced Analytics Queries
- **Objetivo**: Queries complejas y optimización
- **Tecnologías**: PostgreSQL, complex JOINs, CTEs
- **Resultado**: Suite de queries analíticas

```sql
-- projects/05-advanced-queries/analytics.sql
-- Advanced SQL Queries for TaskDB Pro Analytics

-- 1. Project Performance Dashboard Query
WITH project_metrics AS (
    SELECT 
        p.id as project_id,
        p.name as project_name,
        COUNT(t.id) as total_tasks,
        COUNT(CASE WHEN t.status = 'done' THEN 1 END) as completed_tasks,
        COUNT(CASE WHEN t.status IN ('todo', 'in_progress', 'review') THEN 1 END) as active_tasks,
        COUNT(CASE WHEN t.due_date < CURRENT_TIMESTAMP AND t.status != 'done' THEN 1 END) as overdue_tasks,
        AVG(CASE WHEN t.status = 'done' THEN 
            EXTRACT(EPOCH FROM (t.completed_at - t.created_at)) / 3600 
        END) as avg_completion_hours,
        SUM(t.actual_hours) as total_hours_logged,
        SUM(t.estimated_hours) as total_estimated_hours
    FROM projects p
    LEFT JOIN tasks t ON p.id = t.project_id
    WHERE p.is_archived = false
    GROUP BY p.id, p.name
),
velocity_metrics AS (
    SELECT 
        t.project_id,
        DATE_TRUNC('week', t.completed_at) as week_start,
        COUNT(*) as tasks_completed,
        SUM(t.story_points) as story_points_completed
    FROM tasks t
    WHERE t.status = 'done' 
    AND t.completed_at >= CURRENT_DATE - INTERVAL '12 weeks'
    GROUP BY t.project_id, DATE_TRUNC('week', t.completed_at)
)
SELECT 
    pm.*,
    ROUND(
        (pm.completed_tasks::decimal / NULLIF(pm.total_tasks, 0)) * 100, 
        2
    ) as completion_percentage,
    ROUND(
        (pm.total_hours_logged / NULLIF(pm.total_estimated_hours, 0)) * 100,
        2
    ) as estimation_accuracy,
    COALESCE(
        (SELECT AVG(tasks_completed) 
         FROM velocity_metrics vm 
         WHERE vm.project_id = pm.project_id), 
        0
    ) as avg_weekly_velocity
FROM project_metrics pm
ORDER BY pm.completed_tasks DESC;

-- 2. User Productivity Analysis
WITH user_productivity AS (
    SELECT 
        u.id as user_id,
        u.username,
        u.first_name || ' ' || u.last_name as full_name,
        COUNT(t.id) as assigned_tasks,
        COUNT(CASE WHEN t.status = 'done' THEN 1 END) as completed_tasks,
        AVG(CASE WHEN t.status = 'done' THEN 
            EXTRACT(EPOCH FROM (t.completed_at - t.started_at)) / 3600 
        END) as avg_task_completion_hours,
        SUM(te.hours) as total_hours_logged,
        COUNT(DISTINCT t.project_id) as projects_involved
    FROM users u
    LEFT JOIN tasks t ON u.id = t.assignee_id
    LEFT JOIN time_entries te ON u.id = te.user_id
    WHERE u.is_active = true
    GROUP BY u.id, u.username, u.first_name, u.last_name
),
user_rankings AS (
    SELECT 
        *,
        RANK() OVER (ORDER BY completed_tasks DESC) as completion_rank,
        RANK() OVER (ORDER BY total_hours_logged DESC) as hours_rank,
        ROUND(
            (completed_tasks::decimal / NULLIF(assigned_tasks, 0)) * 100,
            2
        ) as completion_rate
    FROM user_productivity
)
SELECT 
    ur.*,
    CASE 
        WHEN ur.completion_rate >= 90 THEN 'Excellent'
        WHEN ur.completion_rate >= 75 THEN 'Good'
        WHEN ur.completion_rate >= 60 THEN 'Average'
        ELSE 'Needs Improvement'
    END as performance_rating
FROM user_rankings ur
WHERE ur.assigned_tasks > 0
ORDER BY ur.completion_rank;

-- 3. Task Bottleneck Analysis
WITH task_flow AS (
    SELECT 
        t.id,
        t.title,
        t.status,
        t.created_at,
        t.started_at,
        t.completed_at,
        p.name as project_name,
        u.username as assignee,
        EXTRACT(EPOCH FROM (COALESCE(t.started_at, CURRENT_TIMESTAMP) - t.created_at)) / 3600 as hours_to_start,
        EXTRACT(EPOCH FROM (COALESCE(t.completed_at, CURRENT_TIMESTAMP) - COALESCE(t.started_at, t.created_at))) / 3600 as hours_in_progress,
        CASE 
            WHEN t.due_date IS NOT NULL AND t.completed_at IS NOT NULL THEN
                EXTRACT(EPOCH FROM (t.completed_at - t.due_date)) / 3600
            WHEN t.due_date IS NOT NULL AND t.status != 'done' THEN
                EXTRACT(EPOCH FROM (CURRENT_TIMESTAMP - t.due_date)) / 3600
            ELSE NULL
        END as hours_vs_deadline
    FROM tasks t
    JOIN projects p ON t.project_id = p.id
    LEFT JOIN users u ON t.assignee_id = u.id
    WHERE t.created_at >= CURRENT_DATE - INTERVAL '90 days'
),
bottleneck_analysis AS (
    SELECT 
        project_name,
        status,
        COUNT(*) as task_count,
        AVG(hours_in_progress) as avg_hours_in_status,
        PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY hours_in_progress) as median_hours_in_status,
        PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY hours_in_progress) as p95_hours_in_status
    FROM task_flow
    WHERE status != 'done'
    GROUP BY project_name, status
)
SELECT 
    *,
    CASE 
        WHEN avg_hours_in_status > 72 THEN 'High Bottleneck'
        WHEN avg_hours_in_status > 24 THEN 'Medium Bottleneck'
        ELSE 'Normal Flow'
    END as bottleneck_severity
FROM bottleneck_analysis
ORDER BY avg_hours_in_status DESC;

-- 4. Dependency Impact Analysis
WITH RECURSIVE task_dependencies_recursive AS (
    -- Base case: tasks without dependencies
    SELECT 
        t.id,
        t.title,
        t.status,
        t.project_id,
        0 as dependency_level,
        ARRAY[t.id] as dependency_path
    FROM tasks t
    WHERE NOT EXISTS (
        SELECT 1 FROM task_dependencies td 
        WHERE td.task_id = t.id
    )
    
    UNION ALL
    
    -- Recursive case: tasks with dependencies
    SELECT 
        t.id,
        t.title,
        t.status,
        t.project_id,
        tdr.dependency_level + 1,
        tdr.dependency_path || t.id
    FROM tasks t
    JOIN task_dependencies td ON t.id = td.task_id
    JOIN task_dependencies_recursive tdr ON td.depends_on_task_id = tdr.id
    WHERE NOT (t.id = ANY(tdr.dependency_path)) -- Prevent cycles
),
dependency_impact AS (
    SELECT 
        t.id,
        t.title,
        t.status,
        p.name as project_name,
        COUNT(blocked.id) as blocks_count,
        MAX(tdr.dependency_level) as max_dependency_level,
        ARRAY_AGG(DISTINCT blocked.title) FILTER (WHERE blocked.id IS NOT NULL) as blocked_tasks
    FROM tasks t
    JOIN projects p ON t.project_id = p.id
    LEFT JOIN task_dependencies td ON t.id = td.depends_on_task_id
    LEFT JOIN tasks blocked ON td.task_id = blocked.id AND blocked.status != 'done'
    LEFT JOIN task_dependencies_recursive tdr ON t.id = tdr.id
    GROUP BY t.id, t.title, t.status, p.name
)
SELECT 
    *,
    CASE 
        WHEN blocks_count > 5 THEN 'Critical Path'
        WHEN blocks_count > 2 THEN 'Important'
        WHEN blocks_count > 0 THEN 'Blocking'
        ELSE 'Independent'
    END as impact_level
FROM dependency_impact
WHERE blocks_count > 0 OR max_dependency_level > 2
ORDER BY blocks_count DESC, max_dependency_level DESC;

-- 5. Time Estimation Accuracy Report
WITH estimation_accuracy AS (
    SELECT 
        t.id,
        t.title,
        t.estimated_hours,
        t.actual_hours,
        p.name as project_name,
        u.username as assignee,
        CASE 
            WHEN t.estimated_hours > 0 THEN
                ROUND(((t.actual_hours - t.estimated_hours) / t.estimated_hours) * 100, 2)
            ELSE NULL
        END as estimation_variance_percent,
        CASE 
            WHEN t.estimated_hours > 0 AND t.actual_hours > 0 THEN
                CASE 
                    WHEN ABS(t.actual_hours - t.estimated_hours) / t.estimated_hours <= 0.1 THEN 'Excellent'
                    WHEN ABS(t.actual_hours - t.estimated_hours) / t.estimated_hours <= 0.25 THEN 'Good'
                    WHEN ABS(t.actual_hours - t.estimated_hours) / t.estimated_hours <= 0.5 THEN 'Fair'
                    ELSE 'Poor'
                END
            ELSE 'No Data'
        END as estimation_quality
    FROM tasks t
    JOIN projects p ON t.project_id = p.id
    LEFT JOIN users u ON t.assignee_id = u.id
    WHERE t.status = 'done' 
    AND t.estimated_hours > 0 
    AND t.actual_hours > 0
),
accuracy_summary AS (
    SELECT 
        project_name,
        assignee,
        COUNT(*) as completed_tasks,
        AVG(estimation_variance_percent) as avg_variance_percent,
        STDDEV(estimation_variance_percent) as variance_stddev,
        COUNT(CASE WHEN estimation_quality IN ('Excellent', 'Good') THEN 1 END) as accurate_estimates,
        COUNT(CASE WHEN estimation_variance_percent > 0 THEN 1 END) as over_estimates,
        COUNT(CASE WHEN estimation_variance_percent < 0 THEN 1 END) as under_estimates
    FROM estimation_accuracy
    GROUP BY project_name, assignee
)
SELECT 
    *,
    ROUND(
        (accurate_estimates::decimal / completed_tasks) * 100,
        2
    ) as accuracy_percentage,
    CASE 
        WHEN avg_variance_percent > 50 THEN 'Consistent Over-estimation'
        WHEN avg_variance_percent < -30 THEN 'Consistent Under-estimation'
        WHEN ABS(avg_variance_percent) <= 10 THEN 'Well Calibrated'
        ELSE 'Needs Improvement'
    END as estimation_pattern
FROM accuracy_summary
WHERE completed_tasks >= 5
ORDER BY accuracy_percentage DESC;

-- 6. Real-time Dashboard Query (Materialized View)
CREATE MATERIALIZED VIEW dashboard_metrics AS
WITH current_metrics AS (
    SELECT 
        COUNT(*) as total_active_tasks,
        COUNT(CASE WHEN status = 'todo' THEN 1 END) as todo_tasks,
        COUNT(CASE WHEN status = 'in_progress' THEN 1 END) as in_progress_tasks,
        COUNT(CASE WHEN status = 'review' THEN 1 END) as review_tasks,
        COUNT(CASE WHEN due_date < CURRENT_TIMESTAMP AND status != 'done' THEN 1 END) as overdue_tasks,
        COUNT(CASE WHEN due_date BETWEEN CURRENT_TIMESTAMP AND CURRENT_TIMESTAMP + INTERVAL '7 days' 
                   AND status != 'done' THEN 1 END) as due_this_week,
        AVG(CASE WHEN status = 'in_progress' THEN 
            EXTRACT(EPOCH FROM (CURRENT_TIMESTAMP - started_at)) / 3600 
        END) as avg_hours_in_progress
    FROM tasks
    WHERE created_at >= CURRENT_DATE - INTERVAL '90 days'
),
weekly_completion AS (
    SELECT 
        DATE_TRUNC('week', completed_at) as week_start,
        COUNT(*) as tasks_completed,
        SUM(story_points) as story_points_completed
    FROM tasks
    WHERE completed_at >= CURRENT_DATE - INTERVAL '12 weeks'
    GROUP BY DATE_TRUNC('week', completed_at)
    ORDER BY week_start DESC
    LIMIT 1
)
SELECT 
    cm.*,
    COALESCE(wc.tasks_completed, 0) as tasks_completed_this_week,
    COALESCE(wc.story_points_completed, 0) as story_points_this_week,
    CURRENT_TIMESTAMP as last_updated
FROM current_metrics cm
CROSS JOIN weekly_completion wc;

-- Create refresh function for materialized view
CREATE OR REPLACE FUNCTION refresh_dashboard_metrics()
RETURNS void AS $$
BEGIN
    REFRESH MATERIALIZED VIEW dashboard_metrics;
END;
$$ LANGUAGE plpgsql;

-- Schedule automatic refresh (requires pg_cron extension)
-- SELECT cron.schedule('refresh-dashboard', '*/5 * * * *', 'SELECT refresh_dashboard_metrics();');
```

#### Video 2.2: Window Functions y Analytics (15 min)
**Mini-Proyecto**: Time Series Analytics
- **Objetivo**: Análisis temporal avanzado
- **Tecnologías**: Window functions, time series
- **Resultado**: Dashboard de métricas temporales

#### Video 2.3: Stored Procedures y Functions (15 min)
**Mini-Proyecto**: Business Logic in Database
- **Objetivo**: Lógica de negocio en BD
- **Tecnologías**: PL/pgSQL, triggers, functions
- **Resultado**: Stored procedures para TaskDB

#### Video 2.4: Query Performance Tuning (15 min)
**Mini-Proyecto**: Query Optimization Lab
- **Objetivo**: Optimización de performance
- **Tecnologías**: EXPLAIN, indexes, query plans
- **Resultado**: Queries optimizadas para producción

---

### **Sección 3: NoSQL Deep Dive - MongoDB (4 videos)**

#### Video 3.1: MongoDB Aggregation Pipeline (15 min)
**Mini-Proyecto**: MongoDB Analytics Engine
- **Objetivo**: Agregaciones complejas en MongoDB
- **Tecnologías**: Aggregation pipeline, MapReduce
- **Resultado**: Sistema de analytics NoSQL

#### Video 3.2: MongoDB Indexing y Performance (15 min)
**Mini-Proyecto**: MongoDB Optimization
- **Objetivo**: Optimización de MongoDB
- **Tecnologías**: Compound indexes, text search
- **Resultado**: MongoDB optimizado para producción

#### Video 3.3: MongoDB Replication y Sharding (15 min)
**Mini-Proyecto**: MongoDB Cluster Setup
- **Objetivo**: Alta disponibilidad MongoDB
- **Tecnologías**: Replica sets, sharding
- **Resultado**: Cluster MongoDB escalable

#### Video 3.4: MongoDB Change Streams (15 min)
**Mini-Proyecto**: Real-time Data Sync
- **Objetivo**: Sincronización en tiempo real
- **Tecnologías**: Change streams, reactive programming
- **Resultado**: Sistema de sync en tiempo real

---

### **Sección 4: Redis y Caching Strategies (3 videos)**

#### Video 4.1: Redis Data Structures y Patterns (15 min)
**Mini-Proyecto**: Advanced Redis Cache
- **Objetivo**: Estructuras de datos Redis
- **Tecnologías**: Redis modules, data structures
- **Resultado**: Sistema de cache avanzado

#### Video 4.2: Redis Pub/Sub y Messaging (15 min)
**Mini-Proyecto**: Real-time Messaging System
- **Objetivo**: Mensajería en tiempo real
- **Tecnologías**: Redis pub/sub, streams
- **Resultado**: Sistema de mensajería escalable

#### Video 4.3: Redis Cluster y High Availability (15 min)
**Mini-Proyecto**: Redis Production Setup
- **Objetivo**: Redis en producción
- **Tecnologías**: Redis cluster, sentinel
- **Resultado**: Redis cluster de alta disponibilidad

---

### **Sección 5: Graph Databases - Neo4j (3 videos)**

#### Video 5.1: Neo4j Graph Modeling (15 min)
**Mini-Proyecto**: Task Relationship Graph
- **Objetivo**: Modelado de grafos
- **Tecnologías**: Neo4j, Cypher, graph patterns
- **Resultado**: Grafo de relaciones de tareas

#### Video 5.2: Cypher Advanced Queries (15 min)
**Mini-Proyecto**: Graph Analytics
- **Objetivo**: Análisis de grafos
- **Tecnologías**: Cypher, path finding, centrality
- **Resultado**: Analytics de relaciones complejas

#### Video 5.3: Neo4j Performance y Scaling (15 min)
**Mini-Proyecto**: Production Graph Database
- **Objetivo**: Neo4j en producción
- **Tecnologías**: Indexing, clustering
- **Resultado**: Base de datos de grafos escalable

---

### **Sección 6: Time Series y Analytics Databases (3 videos)**

#### Video 6.1: ClickHouse Setup y Data Modeling (15 min)
**Mini-Proyecto**: Analytics Warehouse
- **Objetivo**: Data warehouse para analytics
- **Tecnologías**: ClickHouse, columnar storage
- **Resultado**: Warehouse de alto rendimiento

#### Video 6.2: Time Series Data Patterns (15 min)
**Mini-Proyecto**: Metrics Collection System
- **Objetivo**: Recolección de métricas
- **Tecnologías**: Time series patterns, aggregations
- **Resultado**: Sistema de métricas en tiempo real

#### Video 6.3: Real-time Analytics Pipeline (15 min)
**Mini-Proyecto**: Live Analytics Dashboard
- **Objetivo**: Pipeline de analytics en vivo
- **Tecnologías**: Streaming analytics, materialized views
- **Resultado**: Dashboard de métricas en tiempo real

---

### **Sección 7: Multi-Database Architecture (4 videos)**

#### Video 7.1: Polyglot Persistence Strategy (15 min)
**Mini-Proyecto**: Multi-DB TaskPro Architecture
- **Objetivo**: Arquitectura multi-base de datos
- **Tecnologías**: Database per service, CQRS
- **Resultado**: Arquitectura polyglot robusta

#### Video 7.2: Data Synchronization Patterns (15 min)
**Mini-Proyecto**: Data Sync Engine
- **Objetivo**: Sincronización entre BDs
- **Tecnologías**: CDC, event sourcing
- **Resultado**: Motor de sincronización automática

#### Video 7.3: API Gateway for Data Access (15 min)
**Mini-Proyecto**: Unified Data API
- **Objetivo**: API unificada para múltiples BDs
- **Tecnologías**: API Gateway, data federation
- **Resultado**: API unificada de datos

#### Video 7.4: Distributed Transactions (15 min)
**Mini-Proyecto**: Saga Pattern Implementation
- **Objetivo**: Transacciones distribuidas
- **Tecnologías**: Saga pattern, 2PC, eventual consistency
- **Resultado**: Sistema de transacciones distribuidas

---

### **Sección 8: Backup, Recovery y Monitoring (4 videos)**

#### Video 8.1: Backup Strategies y Automation (15 min)
**Mini-Proyecto**: Automated Backup System
- **Objetivo**: Backup automatizado
- **Tecnologías**: pg_dump, mongodump, automation
- **Resultado**: Sistema de backup robusto

#### Video 8.2: Disaster Recovery Planning (15 min)
**Mini-Proyecto**: DR Implementation
- **Objetivo**: Plan de recuperación
- **Tecnologías**: Point-in-time recovery, replication
- **Resultado**: Plan de recuperación probado

#### Video 8.3: Database Monitoring y Alerting (15 min)
**Mini-Proyecto**: Database Observatory
- **Objetivo**: Monitoreo comprehensivo
- **Tecnologías**: Prometheus, Grafana, alerting
- **Resultado**: Sistema de monitoreo completo

#### Video 8.4: Performance Monitoring y Tuning (15 min)
**Mini-Proyecto**: Performance Dashboard
- **Objetivo**: Optimización continua
- **Tecnologías**: Performance metrics, automated tuning
- **Resultado**: Dashboard de performance optimizado

---

### **Sección 9: Data Migration y ETL (4 videos)**

#### Video 9.1: SQL to NoSQL Migration (15 min)
**Mini-Proyecto**: PostgreSQL to MongoDB Migration
- **Objetivo**: Migración entre paradigmas
- **Tecnologías**: Data transformation, migration tools
- **Resultado**: Pipeline de migración automática

#### Video 9.2: ETL Pipeline Design (15 min)
**Mini-Proyecto**: Data Pipeline System
- **Objetivo**: Pipeline ETL robusto
- **Tecnologías**: Apache Airflow, data validation
- **Resultado**: Pipeline de datos automatizado

#### Video 9.3: Real-time Data Streaming (15 min)
**Mini-Proyecto**: Stream Processing System
- **Objetivo**: Procesamiento en tiempo real
- **Tecnologías**: Kafka, stream processing
- **Resultado**: Sistema de streaming de datos

#### Video 9.4: Data Quality y Validation (15 min)
**Mini-Proyecto**: Data Quality Framework
- **Objetivo**: Calidad de datos
- **Tecnologías**: Data validation, quality metrics
- **Resultado**: Framework de calidad de datos

---

### **Sección 10: Production y DevOps (4 videos)**

#### Video 10.1: Database DevOps y CI/CD (15 min)
**Mini-Proyecto**: Database CI/CD Pipeline
- **Objetivo**: DevOps para bases de datos
- **Tecnologías**: Flyway, Liquibase, automation
- **Resultado**: Pipeline CI/CD para BD

#### Video 10.2: Container Orchestration (15 min)
**Mini-Proyecto**: Kubernetes Database Deployment
- **Objetivo**: Orquestación de contenedores
- **Tecnologías**: Kubernetes, operators, StatefulSets
- **Resultado**: Deployment de BD en Kubernetes

#### Video 10.3: Security y Compliance (15 min)
**Mini-Proyecto**: Database Security Framework
- **Objetivo**: Seguridad comprehensiva
- **Tecnologías**: Encryption, access control, auditing
- **Resultado**: Framework de seguridad completo

#### Video 10.4: Cost Optimization y Cloud Management (15 min)
**Mini-Proyecto**: Cloud Database Optimization
- **Objetivo**: Optimización de costos
- **Tecnologías**: Cloud databases, cost monitoring
- **Resultado**: Estrategia de optimización de costos

---

## 🎁 Materiales Descargables Completos

### 📦 Código y Scripts:
1. **Multi-DB Docker Stack** - Stack completo con todas las BD
2. **TaskDB Pro Schema** - Esquemas completos SQL y NoSQL
3. **Migration Scripts** - Scripts de migración entre BD
4. **Performance Optimization** - Scripts de optimización
5. **Backup Automation** - Sistema de backup completo
6. **Monitoring Setup** - Configuración de monitoreo
7. **Security Configurations** - Configuraciones de seguridad

### 🔧 Herramientas y Configuraciones:
1. **Database Comparison Matrix** - Comparativa completa de BD
2. **Performance Benchmarks** - Benchmarks de todas las BD
3. **Migration Toolkits** - Herramientas de migración
4. **Monitoring Dashboards** - Dashboards para Grafana
5. **Security Checklists** - Listas de verificación
6. **Troubleshooting Guides** - Guías de solución de problemas
7. **Cost Optimization Calculator** - Calculadora de costos

### 📚 Documentación Extendida:
1. **Database Architecture Guide** - 200 páginas sobre arquitectura
2. **SQL Performance Bible** - Guía completa de optimización SQL
3. **NoSQL Patterns Handbook** - Patrones y mejores prácticas NoSQL
4. **Data Modeling Masterclass** - Guía de modelado de datos
5. **Production Deployment Guide** - Guía de deployment en producción
6. **Security Best Practices** - Manual de seguridad de BD
7. **Troubleshooting Encyclopedia** - Enciclopedia de solución de problemas

---

## 🏆 Proyecto Final: TaskDB Pro Hybrid System

### Arquitectura Multi-Database:
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   PostgreSQL    │    │    MongoDB      │    │     Redis       │
│   (OLTP Data)   │    │  (Flexible Doc) │    │    (Cache)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
    ┌─────────────────────────────┼─────────────────────────────┐
    │                             │                             │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    ClickHouse   │    │     Neo4j       │    │   Cassandra     │
│   (Analytics)   │    │  (Relationships)│    │  (Time Series)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Features Implementadas:
- ✅ **PostgreSQL**: OLTP data, transacciones ACID
- ✅ **MongoDB**: Documentos flexibles, configuraciones
- ✅ **Redis**: Cache, sessions, real-time data
- ✅ **ClickHouse**: Data warehouse y analytics
- ✅ **Neo4j**: Relaciones y dependencias complejas
- ✅ **Cassandra**: Time series y logs
- ✅ **Data Sync Engine**: Sincronización automática
- ✅ **API Gateway**: Acceso unificado
- ✅ **Monitoring Stack**: Observabilidad completa
- ✅ **Backup System**: Backup automático de todas las BD

### Performance Targets:
- **PostgreSQL**: 10,000+ TPS, <1ms latencia
- **MongoDB**: 100,000+ docs/sec read
- **Redis**: <0.1ms latencia cache
- **ClickHouse**: 1M+ rows/sec analytics
- **Neo4j**: Complex graph queries <100ms
- **99.99% uptime** con alta disponibilidad

---

## 📊 Métricas de Éxito del Curso

### Technical Mastery:
- **SQL Advanced**: 95% dominan queries complejas
- **NoSQL Expertise**: 90% manejan múltiples paradigmas
- **Performance Tuning**: 85% optimizan BD para producción
- **Architecture Design**: 80% diseñan sistemas multi-BD
- **Production Skills**: 75% deployean BD en producción

### Career Impact:
- **Database Roles**: 70% califican para roles de BD
- **Data Engineer Positions**: 50% transicionan a data engineering
- **Architecture Roles**: 40% avanzan a roles de arquitectura
- **Salary Increase**: 55% reportan incremento significativo
- **Consultant Opportunities**: 30% inician como consultores

### Industry Recognition:
- **Portfolio Quality**: 98% tienen TaskDB Pro funcional
- **Interview Performance**: 85% mejoran en entrevistas técnicas
- **Production Experience**: 70% tienen experiencia práctica
- **Multi-DB Expertise**: 80% manejan arquitecturas complejas

Este curso establecerá el estándar de oro para educación en bases de datos, preparando arquitectos de datos para el futuro de la industria.